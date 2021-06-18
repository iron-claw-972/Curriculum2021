# PID

[Press here to return to main page.](https://github.com/iron-claw-972/Curriculum2020)

## 1. Purpose

This activity will introduce the PID, a useful tool in precisely controlling motors. Furthermore, this activity will describe tuning PIDs.

## 2. Materials

- FRC VS Code
  - Downloading FRC VS Code is detailed [here](https://github.com/iron-claw-972/Curriculum2020/blob/master/InstallingFrcPrereqs.md#frc-vscode).
- Access to the device, system you are attempting to control
  - Most common example is a motor
- Something that measures the state of the device or system you are trying to control
  - Most common example is an encoder, which reports the true speed of a motor

## 3. Instructions

### When is PID useful/applied?

PID is used whenever we need to set a variable (a motor's speed, the robot's position, the angle of a robotic arm) on the robot to a particular value, but the variable's value doesn't remain stable or is inaccurate (a motor's speed starts to shift and change, the robot doesn't park in the desired place, the robotic arm is at the wrong angle). PID is a control system, which means it controls the variable so that it remains roughly in place. 

### Creating a PID object

Since we have already written a ready-to-use PID object, how to write a PID object from scratch will be covered later. 
The constructor of a PID object should look like so:

```java

public PID(double pFactor_, double iFactor_, double dFactor_) {
	pFactor = pFactor_;
	iFactor = iFactor_;
	dFactor = dFactor_;
}

```

As you can see, there are 3 "factors". Changing these values dictates the behavior of the PID.
Hence, initializing/declaring one looks like this:

```java

import frc.robot.util.*;

public class SubsystemWithPID {

  public PID example;
  
  //Filled in during tuning period
  private final pFactor;
  private final dFactor;
  private final iFactor;
  
  public SubsystemWithPID() {
    example = new PID(pFactor, iFactor, dFactor);
  }
  
}

```

### Using a PID object

This is best explained through an example. In this case, it will be attempting to stabilize a motor's speed with an encoder, but this can be replaced with any variable and device. The real meat of PID is accomplished in what our ready-made defines as the update method, shown here:

```java
public double update(double setpoint, double actual, double timeFrame) {
	//Actual PID math
	double present = setpoint - actual;
	integral += present * timeFrame;
	double deriv = (present - lastError) / timeFrame;
	lastError = present;
	return present * pFactor + integral * iFactor + deriv * dFactor;
}
```

As you can see, it takes three parameters, the setpoint (meaning the value you want the motor speed [variable] to be), the actual (meaning the value the motor speed is measured to be from the encoder), and the timeFrame (the time from the previous robot update cycle to the next). In the example below, the desired is passed as a parameter to the loop() method, the actual is taken from the encoder (integrated in a TalonFX, changes from motor to motor), and timeFrame is calculated with System.currentTimeMillis().

```java

import frc.robot.util.*;
import com.ctre.phoenix.motorcontrol.can.TalonFX;

public class StableMotorSpeed {

  private final TICKS_PER_ROTATION = Context.TICKS_PER_ROTATION; 
  //How many counts an encoder makes for a revolution; For a TalonFX this is 2048

  public TalonFX motor;
  
  //Your motor's ID of choice
  private final motorID;
  
  public PID motorPID;
  
  //Filled in during tuning period
  private final pFactor;
  private final dFactor;
  private final iFactor;
  
  public double nextInput; //Output from PID
  
  public double actualSpeed; //In RPM
  public double desiredSpeed;
  
  public long currentTime; //Time of update in milliseconds
  public long previousTime; //Time on last update in milliseconds
  public double timeFrame; //Time since last update in seconds
  
  public StableMotorSpeed() {
    	motorPID = new PID(pFactor, iFactor, dFactor);
    	motor = new TalonFX(motorID);
	motor.configSelectedFeedbackSensor(FeedbackDevice.IntegratedSensor);
  }
  
  public loop(double desiredSpeed_) {
  	desiredSpeed = desiredSpeed_;
	actualSpeed = motor.getSelectedSensorVelocity()/TICKS_PER_ROTATION; //converting speed in encoder ticks per second to RPM
	currentTime = System.currentTimeMillis();
	timeFrame = (currentTime - previousTime)/1000.0; //1000 for milliseconds
	previousTime = currentTime;
	nextInput = motorPID.update(desiredSpeed, actualSpeed, timeFrame);
	motor.set(ControlMode.PercentOutput, nextInput);
	
	//Here, we are feeding in nextInput in as the actual proportion of the max current (read: max RPM).
	//However, it may even be preferable to have the PID return the actual next RPM, and 
	//then convert that into a current input.
  }
  
}

```

### Tuning a PID Object

Tuning means to 

After implementing PID, you need to tune it: that is, you need to calibrate it to control the particular variable. It makes sense that the PID you would use for keeping a motor speed stable would be somewhat different from the PID you would use to keep an elevator in place.

In the above example, there are three "factor" variables.

```java
  private final pFactor;
  private final dFactor;
  private final iFactor;
```

Set them all to 0. This will make PID have no impact on the system. In the context of a motor, this means it won't try and rev to the desired speed. 

Here is the basic approach to tuning PID:
1. Set all factors to 0.
1. Increase pFactor by a small amount .
	1. In the context of the example, since we're passing a PercentOutput current from 0 to 1, a good starting amount is ~0.02. The larger pFactor is, the faster the system will move to the desired value (but more recklessly).
1. Turn the system on and observe it. It should slowly move to the desired value.
	1. The motor should slowly rev up to the desired speed.
1. Repeat steps 2, 3 until the system begins to oscillate around the desired variable.
	1. Oscillate means to alternate above or under the desired value but not stay on it.
	1. The motor will be alternating between speeds above and under the desired value consistently.
1. Decrease dFactor by a small amount. Turn the system on and repeat this step until the system stops oscillating.
	1. In the example, start by setting it to around -0.02.
	1. dFactor is comparable to a brake. When pFactor is too high, dFactor can provide stability without dropping too much speed.
1. Go back to step 2. Repeat all of the steps, alternating between increasing pFactor, checking for oscillations, reducing dFactor to stop them, and again until pFactor is large enough that decreasing dFactor no longer reliably reduces oscillations without losing too much speed.
	1. If the system reaches the desired value reliably enough at this point, you can skip the next steps.
1. If the system stops just short or above the desired value, slowly increase iFactor in miniscule steps until this stops happening.
	1. In the context of the example, miniscule is in the neighborhood of ~0.001.

### Explaining PID

You have learned how to use PID without being given any understanding of the math or gears behind it. This will hopefully give you insight into how PID was conceived. 

The output of PID can be understood as:

**Out = pFactor * P + iFactor * I + dFactor * D**

All of the factors are coefficients (fixed numbers) calibrated by the user, you, to fit the system needing control.

* **Error = setpoint - actual**

Error is the basic measure of how well the PID is doing its job; PID's goal is to get this value as close as possible to 0. Take note that when the actual value is less than the setpoint the error is positive, and when greater Error is negative. Assume that a positive PID output is telling the actual value to become greater.

* **P = Error**

This is the most straightforward component of PID. If the actual value is below the desired value, the error is positive, and we add a positive value to the out. Conversely, if we overshoot, the error is negative, and the P term says "go back". The greater the error, the bigger the push is towards the desired value. Imagine this as running fast at the beginning of a marathon and crawling slowly to the end: your error from the end is greatest at the start, so you push yourself harder there.

* **dt = current time - time at last update [in seconds]**

This is basically the time from the last update to the current update. This is needed to understand D and I.

* **D = (Error - lastError)/dt**

lastError is the Error on the last update of the PID loop. So, Error - lastError refers to the change in Error from the last update, which means we are dividing the change in error by the change in time. If that sounds like the slope formula for a line at all, then you are on the right track: we are finding the rate of change of error over time. The greater that value is, the faster actual is approaching the desired value.

Imagine if you were only using P: if pFactor was small, you might steadily reach the desiredValue at a snail's pace. If pFactor was large, you might quickly overshoot the desiredValue and spend unnecessary time correcting the issue.

We mentioned earlier that dFactor is usually negative, which makes dFactor * D analagous to a brake. Take the last scenario and put a brake on it: when D is really high (causing you to overshoot the target), the negative dFactor steps in and subtracts from P to slow it down. This creates a much more controlled PID, where the actual goes at a fairly consistent high speed towards the desired value but doesn't grow too big that P can't slow down.

* **I = (lastI + Error * dt);

Using just P and D (aka iFactor = 0), you can get a really fast PID. However, when pushing pFactor and dFactor far enough, it sometimes occurs that the brake will wind up bringing the system to a halt just before or after the desired value, preventing it from going further.

This results in a consistent, final error. This means we need something grows larger the longer an error exists. Looking up at the formula, if there is a consistent error over a long enough period of time, I will keep growing and eventually becomes large enough that it can overcome the brake, reaching the finish line. In the context of PID, the dt appropriate scales the error to the time passed.

iFactor is kept small because we only want I to show its influence at the very end of the adjustment. When iFactor is made larger, I begins to quickly show its influence and can create some pretty spastic behavior in the PID that you don't want. 

If a PD system is doing fine without an I term, there's no need for it; simpler is smarter. In theory, a control system is possible with just an I term, but this is vastly inferior to just a PD loop, although perhaps appropriate in certain cases (judged as they come).

### PID in Code

https://github.com/iron-claw-972/FRC2020/blob/master/src/main/java/frc/robot/util/PID.java

The constructor and edge shine are intuitive; what's important is the update method.

```java

public double pFactor, iFactor, dFactor;
//Integral Sum
public double integral;
//The previous error for derivative calculation
private double lastError;

//Constructor was here

//Runs through one step of the PID
public double update(double setpoint, double actual, double timeFrame) {
	//Actual PID math
	double present = setpoint - actual; 
	integral += present * timeFrame;
	double deriv = (present - lastError) / timeFrame;
	lastError = present;
	return present * pFactor + integral * iFactor + deriv * dFactor;
}
```

The error is the double present. 
The I term sum is the double integral.
The D term is the double derivative.
dt is timeframe, desired value is setpoint, actual is actual, etc.

The calculations described are intuitively seen in the code; try and discern them.

### Graphing to Tune Better

///

## 4. Further Reading

[JanisMac's Control Challenges](https://janismac.github.io/ControlChallenges/) are very simple JavaScript based challenges that are made to be solved with PID control systems.
