---
title: Improving in-game power supplies
description: ""
date: 2026-03-23T13:11:38.992Z
preview: ""
draft: true
tags: []
categories: []
---
From the outset I wanted Itinerant to have modularised technology that relied on proper object-oriented programming principles rather than hard coding things to happen in-game. For example, if you want to switch a computer on and use it, it will need to have a power supply. What I didn't want to do was have some sort of 'pretend' system where you kind of fake a power supply being connected. Probably the easy way of doing this would be to tag an object as being 'PowerSupply' and then if you connect it do a computer, the computer recognises the 'PowerSupply' tag and switches on.

Having now written that down, I'm beginning to feel like that doesn't seem like a half bad idea. What I've done instead is somewhat more cmoplex.

I wanted things in game to be able to figure out for themselves if they can be connected together and what the net result would be. Somebody could maybe design a solar panel with a power output, but if it doesn't generate enough power, the computer won't be able to switch on unless you daisy-chain enough solar panels together.

In other real-world projects I've worked on, I've had systems where a signal can be sent from one device to another. The signal itself is actually packets of data where a definition exists explaining what data is being sent. Every time the data changes in some way, it gets forward to whatever it's connected to. The two devices can be developed in complete isolation from one another; so long as they follow the same protocol, they'll be able to operate together.

In my early tests I set up a really simple power supply method that I've been using up until now. If we look at a very basic example of connecting a portable generator to a light bulb, there are two things working together:

1. We have the power supply. The internal components look like this:

![Internal connections within an example power generator](/posts/2026-03-23-generator-diagram.png)

Since this was a first test, I kept things simple and created an 'InfinitePowerSource' that always provides a 'Charge'. The code on the backend looks like this:

```
public class InfinitePowerSource : ComponentBase    // Every component needs to inherit from ComponentBase
{
    // The amount of 'charge' the device can give, the maximum being '1'.
    // The 'ComponentOutput' attribute tells Itinerant that this property can be used as an output.
    [ComponentOutput]
    public float Charge => 1.0f;

    public override void Dispose()
    {
        
    }
}
```

This is of course oversimplified.

2. At the other end we have the light bulb, which can take a Charge as an input:

![A light bulb with a power connection](/posts/2026-03-23-lightbulb-diagram.png)

I'll have to explain what's going on here in more detail aonther time, but the basics of it is that you can define parts of the object's 3D mesh to act as a 'passthrough', connecting them to internal components of an object (in this case, the light emitter). In this case, the light builb has one mesh which defines where the charge is brought in from, and another mesh which defines where the light is emitted from. The idea behind this is that if a designer creates a new light, it should be relatively straightforward for them to hook up a generic 'light-bulb' component and know it'll start working in game without the need to code anything new up.

The code for the light bulb is more complex, but the main part that interests us for now is this:

```
private float _charge = 0f;
[ComponentInput]
public virtual float Charge
{
    get
    {
        return _charge;
    }
    set
    {
        if (_charge != value)
        {
            _charge = value;
            if (_charge > 0.2f)
            {
                _light.Illuminated = true;
            }
            else
            {
                _light.Illuminated = false;
            }
        }
        OnPropertyChanged();
    }
}
```

So we can see that if the user connects the InfinitePowerSupply's 'charge' property to a 'charge' property on the light builb, it'll recieve the value of '1' - enough to make thee light switch on.

So this has all been fine while I've been testing, but I was working on getting moving vehicles in game recently and I realised I need something that allows me to hook up batteries to the vehicle to power the vehicle, and the batteries should drain over time.

I initially thought this would be super easy and my design would cope with it easily, but it almost immediately became a bit of a stumbling block.

- How do I know how much energy the light bulb is draining from the battery at any one time?
- How do I know if the battery is providing enough power to keep the light rumming?
- How do we handle things when the battery runs out of energy?

First of all, I thought I'd have to just create more properties - have an output coming from the light bulb back to the battery containing a float that tells it how much. This soon became somewhat difficult to manage in game because of course it meant having to set up at least two connections between the battery and the light builb to get an idea of how power was being transferred from the battery to the light bullb.

Arguably you could say this is fairly close to reality, as you need two connections tho light a light buil up (you'll need a positive and a negative signal). But it doesn't make sense from a user experience perspective, unless you're an electrician, that you are connecting an output from the battery to the light bulb and then another output from the light bulb back to the battery. Normal every day users are accustomed to having one cable that contains several wires, and you just connect up that one cable for everything to work.

This made me realise that what I really want is a 'connection group' that can be made up of several inputs and outputs. These connection groups are subcomponents that can contain any number of inputs and outputs, and the whole thing remains transparent to the user, who only sees an output that can be connected to an input. This led me to the following code...

We have a PowerOutput sumcomponent:
```
    public class PowerOutput : ComponentBase, IPowerOut
    {
        private float _powerAvailable;
        public float PowerAvailable
        {
            get
            {
                return _powerAvailable;
            }
            set
            {
                if (value != _powerAvailable)
                {
                    _powerAvailable = value;
                    OnPropertyChanged("PowerProvided");
                }
            }
        }
        private float _powerRequested = 0;
        public float PowerProvided { get => Math.Min(PowerAvailable, _powerRequested); }
        public event EventHandler<float>? OnPowerRequestChanged;

        public float PowerRequested
        {
            get => _powerRequested;
            set
            {
                if (_powerRequested != value)
                {
                    _powerRequested = value;
                    OnPowerRequestChanged?.Invoke(this, value);
                }
            }
        }

        public override void Dispose()
        {
            
        }
    }
```

PowerInput:
```
public class PowerInput : ComponentBase, IPowerIn
{
    //private Action<float>? _onPowerProvidedUpdate;
    public event EventHandler<float> OnPowerProvided_Changed;

    private float _powerRequested;
    private float _powerProvided;
    public float PowerProvided
    {
        get
        {
            return _powerProvided;
        }
        set
        {
            if (_powerProvided != value)
            {
                _powerProvided = value;

                OnPowerProvided_Changed?.Invoke(this, _powerProvided);
            }
        }
    }

    public float PowerRequested
    {
        get => _powerRequested;
        set
        {
            if (value != _powerRequested)
            {
                _powerRequested = value;
                OnPropertyChanged();
            }
        }
    }

    public override void Dispose()
    {

    }

    public PowerInput(float powerRequested)
    {
        _powerRequested = powerRequested;
    }
}
```

So, the PowerOutput has a 'PowerAvailable' (in Watts) property that the parent component (the generator) can set saying how much power is currenly availabls. It also has a 'PowerProvided' output that will go to the device saying how much power it actually delivers, and a 'PowerRequested' input from the device tell us how much power the device actually wants.

Similarly, the PowerInput side has a 'PowerProvided' input connection telling the receiving side how much energy it actually gets, and a 'PowerRequested' output that gets sent to the power source telling it how much power it shuold deliver (if it can).

So now we can have a battery like this:

```
/// <summary>
/// Radioisotope Thermoelectric Generator
/// </summary>
public class RTG : ComponentBase
{
    /// <summary>
    /// The instantaneous amount of power the battery can provide, in Watts, assuming it is fully charged.
    /// </summary>
    [ComponentSetting]
    public float InstantPower { get; set; } = 400f;

    [ComponentOutput(ConnectionGroup: true)]
    public PowerOutput PowerSupply1 { get; set; }

    [ComponentOutput(ConnectionGroup: true)]
    public PowerOutput PowerSupply2 { get; set; }

    [ComponentOutput(ConnectionGroup: true)]
    public PowerOutput PowerSupply3 { get; set; }

    [ComponentOutput(ConnectionGroup: true)]
    public PowerOutput PowerSupply4 { get; set; }

    public RTG()
    {
        PowerSupply1 = new PowerOutput() { PowerAvailable = InstantPower };
        PowerSupply1.OnPowerRequestChanged += PowerSupply_OnPowerRequestChanged;
        PowerSupply2 = new PowerOutput() { PowerAvailable = InstantPower };
        PowerSupply2.OnPowerRequestChanged += PowerSupply_OnPowerRequestChanged;
        PowerSupply3 = new PowerOutput() { PowerAvailable = InstantPower };
        PowerSupply3.OnPowerRequestChanged += PowerSupply_OnPowerRequestChanged;
        PowerSupply4 = new PowerOutput() { PowerAvailable = InstantPower };
        PowerSupply4.OnPowerRequestChanged += PowerSupply_OnPowerRequestChanged;
    }

    private void PowerSupply_OnPowerRequestChanged(object? sender, float e)
    {
        var totalPowerRequired = PowerSupply1.PowerRequested + PowerSupply2.PowerRequested + PowerSupply3.PowerRequested + PowerSupply4.PowerRequested;

        if (totalPowerRequired < InstantPower)
        {
            // The total required is less than the maximum we can provide, so just let all the power be delivered.
            PowerSupply1.PowerAvailable = PowerSupply1.PowerRequested;
            PowerSupply2.PowerAvailable = PowerSupply2.PowerRequested;
            PowerSupply3.PowerAvailable = PowerSupply3.PowerRequested;
            PowerSupply4.PowerAvailable = PowerSupply4.PowerRequested;
        }
        else
        {
            // otherwise we need to distribute whatever power we have across all devices
            var dropFactor = InstantPower / totalPowerRequired;
            PowerSupply1.PowerAvailable = PowerSupply1.PowerRequested * dropFactor;
            PowerSupply2.PowerAvailable = PowerSupply2.PowerRequested * dropFactor;
            PowerSupply3.PowerAvailable = PowerSupply3.PowerRequested * dropFactor;
            PowerSupply4.PowerAvailable = PowerSupply4.PowerRequested * dropFactor;
        }
    }

    public override void Dispose()
    {
        PowerSupply1.OnPowerRequestChanged -= PowerSupply_OnPowerRequestChanged;
        PowerSupply2.OnPowerRequestChanged -= PowerSupply_OnPowerRequestChanged;
        PowerSupply3.OnPowerRequestChanged -= PowerSupply_OnPowerRequestChanged;
        PowerSupply4.OnPowerRequestChanged -= PowerSupply_OnPowerRequestChanged;
    }
}
```

This is a 