---
title: "Virtual Knobs: Making human-machine interaction more intuitive and efficient"
categories: [English]
tags: [Human-Machine Interaction, UI Design, Mobile Development, Automotive Systems]
---

> **Keywords**: Human-Machine Interaction, UI Design, Mobile Development, Automotive Systems, VR/AR 

## Summary

This article introduces an innovative virtual knob gesture system that mimics the physical knob experience. The system calculates virtual knobs and their rotation angles from multi-touch input, leverages knob position and size as extended dimensions, and incorporates improved UI design principles to achieve fast, precise adjustment of wide-range values with strong interference resistance. Successfully implemented in the YAP video player, it demonstrates excellent user experience and broad application potential across desktop, mobile, automotive, VR, and future interaction environments.

## TL;DR
---

## Physical vs. Digital: The Human-Machine Interface Debate

It all started with a casual conversation with my sister in early 2024. We were discussing news about physical knobs making a comeback in electric vehicles. This sparked my curiosity: **why do physical knobs still have a place in this touchscreen-dominated world?**

After diving deep into research, I discovered that touchscreens and physical knobs each have distinct advantages suited for different scenarios.

### The Touch + GUI Paradigm

Modern mobile devices predominantly use touchscreen + GUI designs with these characteristics:

**Strengths:**
- **Intuitive interaction**: Direct manipulation of on-screen elements feels natural.
- **Versatile applications**: Highly flexible design adapts to diverse use cases.
- **Rich functionality**: Supports complex interactions and workflows.

**Limitations:**
- **Visual dependency**: The "locate-then-operate" model consumes significant visual attention (think multi-level menu navigation).
- **High cognitive load**: Same functions often require different interaction patterns across apps.
- **Lack of tactile feedback**: Missing the physical sensations users expect.
- **Poor interference resistance**: Designed for stable desktop/mobile environments without considering vibration or movement.

### Physical Knobs: The Analog Advantage

Traditional knobs offer compelling benefits:

1. **Simple operation**: Perfect for adjusting continuous values
2. **Muscle memory**: Fixed positions enable eyes-free operation
3. **Clear physical boundaries**: Tactile feedback includes resistance and detents
4. **Limited quantity**: 
   - **Cost, complexity, and reliability scaling**: Each additional knob increases manufacturing costs, installation difficulty, maintenance overhead, mechanical failure probability, and space requirements
   - **Cognitive limitations**: Too many knobs reduce muscle memory effectiveness


### Automotive Requirements: A Perfect Storm

Vehicle interfaces demand:
- **Simple operation**
- **Minimal distraction**
- **Tactile feedback**
- **Interference resistance**

The analysis reveals a clear pattern: automotive environments require safe, simple, and interference-resistant interfaces. Traditional desktop/mobile GUIs fall short, while physical knobs excel. However, due to the quantity limitations, physical knobs are typically reserved for critical or high-frequency functions (temperature, volume, etc.). 

**The core challenge** becomes: How can we capture the intuitive benefits of physical knobs without the associated costs and space requirements?
**Solution approach**:
1. Design virtual knobs that simulate physical knob experiences
2. Improve UI design to reduce visual dependency

---

## Initial Design: Mimicing Physical Knobs


### Mathematical Model

I developed a simple mathematical model for virtual knobs. A virtual knob can be completely described by four parameters: **center position c, radius r, rotation angle θ, and corresponding value v**, represented as (c, r, θ, v).

**Angle Calculation**: Knob rotation is calculated through cumulative angle differences:
```
Δθ = θ₁ - θ₀
```

**Value Mapping**: Value changes correlate with angle changes through a linear function:
```
v₁ = v₀ + α × Δθ
```

Where:
- **v₀, v₁**: Values before and after change
- **θ₀, θ₁**: Angles before and after change  
- **α**: Sensitivity parameter controlling angle-to-value conversion ratio

**Touchscreen Implementation Algorithm**:
1. Calculate distances between all touch points, select the two points p₁(x₁, y₁) and p₂(x₂, y₂) with maximum distance as the primary vector (reduces noise impact)
2. Knob center: c = ((x₁+x₂)/2, (y₁+y₂)/2)
3. Knob radius: r = \|p₁p₂\|/2
4. Rotation angle: Calculate Δθ through primary vector angle changes

### Reimagining interaction Design
Traditional UI follows a vision-based "locate-then-operate" paradigm. UI elements occupy certain screen positions, requiring users to visually locate target elements (buttons, menus) before interacting with them (clicking, dragging). While this approach offers intuitive interaction and versatile application scenarios, it also introduces high cognitive load and heavy visual dependency. This design model originated for and remains suitable for desktop and mobile environments — but becomes a serious liability when applied to automotive interfaces.

### Virtual Knob UI Design Principles

To minimize visual dependency and ensure driving safety, I adopted these design principles:

1. **No fixed positions**: Virtual knobs have no fixed position but follow the finger's position, which can reduce visual attention needed for positioning.
2. **Large distinguishable operation areas**: Designate a sufficiently large operation area on the screen (large enough for rotation operations and with redundant space for disturbances like bumps). Any rotation gesture within this operation area is interpreted as knob rotation. The operation area can be distinguished through visual, auditory, and tactile aspects. For example, the operation area uses prominent colors or patterns as background; it emits alert sounds when fingers touch the operation area; it provides continuous vibration feedback during operation within the area, and the vibration stops when fingers move out of the operation area.
3. **Context-aware activation**: Define activation actions (voice wake-up, specific swipe patterns) to activate operation zones. These floating windows auto-dismiss when not needed.
4. **Multi-knob system**: Different activation actions enable different virtual knobs without additional hardware costs. Say "adjust volume" → volume control zone appears; say "adjust seat" → seat control zone appears. Different zones should be clearly distinguishable (different backgrounds, different alert sounds, or different vibration patterns).
5. **Tactile enhancement**: Screen vibration and other haptic feedback technologies increase virtual knob realism and intuitiveness.

### Prototype Results

I spent about two weeks completing the virtual knob prototype on an Android device. I found it remarkably effective - the operation is similar to physical knobs, intuitive and natural; and the performance is excellent, with a smooth experience even on low-end phones, allowing for fast and precise adjustments. Compared to commonly used components like Slider or Stepper for adjusting values on mobile devices and desktop scenarios, Slider is fast but imprecise, while Stepper is precise but slow. And because it uses rotation angle to adjust values, it has natural interference resistance to horizontal and vertical bumps, making it very suitable for automotive and other unstable environments.

---

## Further design: Beyond Physical Knobs

After completing the initial design, I explored additional possibilities. The prototype worked excellently but only used rotation angle for adjusting. According to the mathematical model, Virtual knobs still have two parameters c and r not used  — could these be utilized too?

Through experimentation, I discovered operational precision limits:

| Operation Type | Experimental Range | Practical Precision | Controllable Values |
|---------------|------------------|-------------------|-------------------|
| Rotation | ~100° | 1° minimum interval | ~100 values |
| Scaling | 400-500 pixels | Several pixels | Tens of values |
| Translation | 400-800 pixels | Several pixels | Tens of values |

This approach works well for common applications but falls short in high-precision or wide-range scenarios. Consider timer settings: even when limited to a single day, we have 24×60×60=86,400 seconds. Since single operations can only handle tens to hundreds of values, they cannot efficiently map to such large ranges. Meeting this capacity requirement forces us to either reduce range or lower precision.

The iPhone timer exemplifies this trade-off: it divides time setting across three separate wheels for hours, minutes, and seconds. While each wheel handles only dozens of values, users must perform three separate adjustments, resulting in poor efficiency.

However, Apple's approach also reveals a key insight: by treating hour/minute/second selection as an additional dimension, we can transform a one-dimensional variable with enormous range into a multi-dimensional system with manageable sub-ranges. This principle extends naturally to virtual knobs — we can leverage the unused parameters (center position c and radius r) to introduce new dimensions. I call it **dimension extension**, which dramatically expanding the system's representational capacity.

### Application Case 1: Enhanced Timer Design

Using knob radius to select hours, minutes, or seconds (radius < r₁ -> hours; r₁ ≤ radius ≤ r₂ -> minutes; radius > r₂ -> seconds), with rotation angle adjusting specific values. The knob becomes three concentric rings, with finger scaling enabling easy switching between different rings. One rotation gesture sets time quickly and precisely.

### Application Case 2: Advanced Video Editing

For video editing or surveillance browsing requiring various playback speeds, beyond discrete time unit mapping, continuous functions can be constructed (for simplicity, using a linear function as an example):

```
unit_t = β × r
```

Where:
- **unit_t**: Time unit (time corresponding to unit angle rotation)
- **r**: Knob radius
- **β**: Proportional coefficient 

### Application Case 3: YAP Media Player

In my video player **YAP**, both knob position and size select variables simultaneously. During playback, gestures on the left side use larger virtual knobs (wider finger spread) for volume control and smaller knobs for brightness. Similarly, right-side gestures use large/medium/small knobs corresponding to 1-second/30-second/10-minute seeking increments. This enables rapid and precise navigation to any video position.

**Demo Videos**:
- [Bilibili Demo](https://www.bilibili.com/video/BV1iYeeztECf/?vd_source=58a10372417c06b5b1b6a5946b852c45)
- [YouTube Demo](https://youtu.be/kncUSeDiOe8)

**Download & Try** (Free app, iOS/Android):
- [App Store](https://apps.apple.com/app/y-a-p/id6744711983)
- [Google Play](https://play.google.com/store/apps/details?id=ex3.yap)

### VR/AR Environment Extensions

Since virtual knobs depend only on fingertip positions, they're perfectly suited for VR/AR environments. Through omputer vision or other methods to track fingertip positions in real-time, knob rotations can be calculated. With appropriate activation/deactivation mechanisms, seamless VR/AR operation becomes possible.

---

## Looking Forward: The Future of Interaction

Currently, I am applying for a patent for the knob gesture. I believe that knob gestures, as a fundamental human-machine interaction technology, will be widely applicable across desktop, mobile, VR, and future interaction environments, making human-machine interaction more effective and engaging!

---

## Reflection on Innovation

This article presents the thought process in logical order, but actual innovation is full of leaps and serendipity! The innovation process is arduous and challenging, but the excitement when inspiration strikes and the fulfillment of completing a product give me the drive to keep innovating!

---

p.s.
An introduction video of Virtual Knob: 
- [Searching for the lost gesture - YouTube](https://youtu.be/Ab15RVB1G_U)

## Related Topics

`Human-Machine Interaction` `UI/UX Design` `Mobile Development` `Automotive Systems` `Touch Interaction` `Innovation` `Patents`

---

*Original technical content | Please credit when sharing*