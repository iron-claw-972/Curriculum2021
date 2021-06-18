# Writing Basic Robot Drive Code

[Press here to return to main page.](https://github.com/iron-claw-972/Curriculum2020)

## 1. Purpose

This activity will introduce several key topics, such as how to spin a motor, how to read a joystick, and how to write basic robot drive code. In addition, it will describe how to use some of the features of FRC VS Code, such as creating a robot project with the WPILib extension.

## 2. Materials

- FRC VS Code
  - Downloading FRC VS Code is detailed [here](https://github.com/iron-claw-972/Curriculum2020/blob/master/InstallingFrcPrereqs.md#frc-vscode).
- Github Desktop

## 3. Instructions

# Creating a Project

1. Refer to the Github tutorial and create a new repository. The default location should be in Documents/Github/TheRepo
2. Next open up VSCode 2020. 
3. Click the W icon in the top right corner. This should open up a text box. Type in "Create a new project" and press enter. This should bring you to the WPILib New Project Creator
4. Click "Select a project type (Example or Template)" 
5. Press template, then Java, then Timed Robot
6. Next click "Select a new project folder" and find and select the repository folder you created previously.
7. Enter a valid project name using capital letters to separate words with no spaces (ex. "ExampleProject")
8. I hope you can answer the last question
9. Click generate project
10. Inside github desktop, many files should have appeared. Write a summary and description and click commit to master.


![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/commitcreatechanges.png)


# Making robot move

1. First we will need to install some vendor libraries. Install the ones for CTRE and Rev. It is detailed [here](https://github.com/iron-claw-972/Curriculum2020/blob/master/InstallingFrcPrereqs.md#vendor-libraries)
2. Next navigate to Robot.java by clicking the arrows in the folder structure on the left:

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/clickrobotdotjava.png)

3. Double click on Robot.java. The file editor should open.
4. Since we won't be doing anything with autonomous right now, you can delete everything inside this file and paste this code:

```

package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.Joystick;
import com.revrobotics.CANSparkMax;
import com.ctre.phoenix.motorcontrol.can.*;
import com.ctre.phoenix.motorcontrol.ControlMode;


public class Robot extends TimedRobot {

  @Override
  public void robotInit() {

  }

  @Override
  public void robotPeriodic() {

  }

  @Override
  public void autonomousInit() {

  }

  /**
   * This function is called periodically during autonomous.
   */
  @Override
  public void autonomousPeriodic() {

  }

  /**
   * This function is called periodically during operator control.
   */
  @Override
  public void teleopPeriodic() {
  }

  /**
   * This function is called periodically during test mode.
   */
  @Override
  public void testPeriodic()

  }
}
```


5. Right now, this code doesn't make the robot do anything. In FRC, there are mainly 4 types of motor controllers we use - the VictorSPX, the TalonSRX, the TalonFX, and the SparkMax. Each of these need to be programmed in a different way. Let's go over the basics of how to use each motor controller to spin a motor.

- TalonSRX: To initialize the TalonSRX, first declare an instance variable with the type `TalonSRX` in the Robot class. For example, `TalonSRX TalonSRXMotor;`. Next in the robotInit method, we want to initialize the variable. To initialize a motor controller, we need the CAN Device ID the motor controller is plugged in to. This can be found on the robot. When you don't know what the id will be, you can just leave it at -1 (non-existant id) so people know that the CAN IDs need to be added. Add `TalonSRXMotor = new TalonSRX(-1);` to robotInit. To spin the motor, add `TalonSRXMotor.set(ControlMode.PercentOutput, .9);` to teleop periodic. This will tell the robot to keep spinning the motor at 90% power during teleop. Using -0.9 will cause the motor to spin the other way. The resulting code will look like this:

```
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import com.ctre.phoenix.motorcontrol.ControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonSRX;


public class Robot extends TimedRobot {

  TalonSRX TalonSRXMotor;

  @Override
  public void robotInit() {
    TalonSRXMotor = new TalonSRX(-1);
  }

  @Override
  public void teleopPeriodic() {
    TalonSRXMotor.set(ControlMode.PercentOutput, .9);
  }

}
```
- VictorSPX: The VictorSPX works in the same way as the TalonSRX except it a different class.
```
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import com.ctre.phoenix.motorcontrol.ControlMode;
import com.ctre.phoenix.motorcontrol.can.VictorSPX;


public class Robot extends TimedRobot {

  VictorSPX VictorSPXMotor;

  @Override
  public void robotInit() {
    VictorSPXMotor = new VictorSPX(-1);
  }


  @Override
  public void teleopPeriodic() {
    VictorSPXMotor.set(ControlMode.PercentOutput, .9);
  }

}
```
- TalonFX: The TalonFX also works similar to the TalonSRX except you should use the TalonFXControlMode class:
```
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import com.ctre.phoenix.motorcontrol.TalonFXControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonFX;


public class Robot extends TimedRobot {

  TalonFX TalonFXMotor;

  @Override
  public void robotInit() {
    TalonFXMotor = new TalonFX(-1);
  }


  @Override
  public void teleopPeriodic() {
    TalonFXMotor.set(TalonFXControlMode.PercentOutput, .9);
  }

}
```

- The SparkMax uses the CANSparkMax class as shown below. If you are using the SparkMax with a NEO motor, you MUST plug in an encoder wire as it is brushless. Otherwise the motor will fry. If you are not using a brushless motor make sure to use `MotorType.kBrushed`. To check if a SparkMax is driving a brushed or brushless motor, check how many wires are coming out of it (excluding the encoder wires). There will be three for brushless motors and two for brushed motors.

```
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import com.revrobotics.CANSparkMax;
import com.revrobotics.CANSparkMaxLowLevel.MotorType;

public class Robot extends TimedRobot {

  CANSparkMax SparkMaxMotor;

  @Override
  public void robotInit() {
    SparkMaxMotor = new CANSparkMax(-1, MotorType.kBrushless);
  }


  @Override
  public void teleopPeriodic() {
    SparkMaxMotor.set(.9);
  }

}
```
6. Great, we can spin many types of motors. But how do we get this code onto the robot and run it? First we need to connect our computer to the robot, either with a USB cable, ethernet cable, or through wifi. When using wifi, check the name of the network of the robot (it will be on the bridge), and connect to the wifi network. Then in FRC VSCode (after saving your code), click the W in the upper right hand corner, and select deploy robot code. If the deploy is not successful, it could be due to your code not compiling, not having the correct vendor libraries, not being connected to the robot, or another reason. Make sure to read the error message. If code code deploys successfully, you can open FRC Driver Station, shout ENABLING, and then click enable. If the robot does something unexpected and is not suspended on blocks, you can press disable or the space key to stop it. Note that if you press the space key you will not be able to enable it again until you redeploy.

7. Next we will drive this motor off of a joystick. Controllers have buttons and axis: buttons can give a 0 or 1 value while axis can range from -1 to 1 (except for triggers which go from 0 to 1). In this example we will use an axis from the joystick, which will correspond to how much power we tell a TalonSRX to give a motor. Each joystick has 2 axis, one for how much it is moved left and right and one for how much it is moved up and down. Each axis has an ID which can be determined by opening FRC Driver Station. Additionally, each controller has an ID which can be determined in FRC Driver Station. To use input devices like controllers and gamepads, we have to use the Joystick class (it represents a whole controller, not just a joystick). To get the -1 to 1 value of an axis, we can use the `getRawAxis(int ID)` method, and to get the 0 or 1 value of a button, we can use the `getRawButton(int ID)` method.

```
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import com.ctre.phoenix.motorcontrol.ControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonSRX;
import edu.wpi.first.wpilibj.Joystick;

public class Robot extends TimedRobot {

  TalonSRX TalonSRXMotor;
  Joystick Controller;
  double motorPower;
  double powerMultiplier = .5;

  @Override
  public void robotInit() {
    Controller = new Joystick(0); #using controller 0
    TalonSRXMotor = new TalonSRX(-1);
  }

  @Override
  public void teleopPeriodic() {
    motorPower = Controller.getRawAxis(0) * powerMultiplier; #get the -1 to 1 values from the joystick and half it so the motor runs at a maximum of half speed
    TalonSRXMotor.set(ControlMode.PercentOutput, motorPower);
  }

}
```
8. Now that we can spin a motor using a joystick, we can move onto driving a robot. There are 2 primary ways of doing this on a standard robot with traction or omni wheels. The first is tank drive. In tank drive, the left joystick controls the power of the left wheels, and the right joystick controls the power of the right wheels. To go forward, both joysticks need to be pushed forward, and to turn left, the right joystick needs to be pushed more forward than the left joystick. It is fairly simple to create a tank drive. Again, we will use the TalonSRX as an example. 
```
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import com.ctre.phoenix.motorcontrol.ControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonSRX;
import edu.wpi.first.wpilibj.Joystick;

public class Robot extends TimedRobot {

  TalonSRX TalonSRXMotorLeft;
  TalonSRX TalonSRXMotorRight;
  TalonSRX TalonSRXMotorLeftSlave;
  TalonSRX TalonSRXMotorRightSlave;
  Joystick Controller;
  double motorPower;
  double powerMultiplier = .5;

  @Override
  public void robotInit() {
    Controller = new Joystick(0); #using controller 0
    TalonSRXMotorLeft = new TalonSRX(-1);
    TalonSRXMotorRight = new TalonSRX(-2);

    TalonSRXMotorLeftSlave = new TalonSRX(-3);
    TalonSRXMotorRightSlave = new TalonSRX(-4);

    
  }

  @Override
  public void teleopPeriodic() {
    motorPowerLeft = Controller.getRawAxis(0) * powerMultiplier; #get the -1 to 1 values from the left joystick and half it so the motor runs at a maximum of half speed

    motorPowerRight = Controller.getRawAxis(1) * powerMultiplier; #get the -1 to 1 values from the right joystick and half it

    TalonSRXMotorLeft.set(ControlMode.PercentOutput, motorPowerLeft); #moves according to left joystick
    TalonSRXMotorRight.set(ControlMode.PercentOutput, motorPowerRight); #moves according to right joystick

    TalonSRXMotorLeftSlave.follow(TalonSRXMotorLeft); #rest of the motors follow along
    TalonSRXMotorRightSlave.follow(TalonSRXMotorRight); #follow method works for victors, talons, and the talonFX. SparkMaxs can follow other SparkMaxs
  }

}
```

9. A more commonly used way of driving a robot is arcade drive. In this system, one joystick controls the power or speed of the robot and the other joystick controls the turning of the robot.
```
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import com.ctre.phoenix.motorcontrol.ControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonSRX;
import edu.wpi.first.wpilibj.Joystick;

public class Robot extends TimedRobot {

  TalonSRX TalonSRXMotorLeft;
  TalonSRX TalonSRXMotorRight;
  TalonSRX TalonSRXMotorLeftSlave;
  TalonSRX TalonSRXMotorRightSlave;
  Joystick Controller;
  double motorPower;
  double leftJoyValue;
  double rightJoyValue;

  @Override
  public void robotInit() {
    Controller = new Joystick(0); #using controller 0
    
    TalonSRXMotorLeft = new TalonSRX(-1);
    TalonSRXMotorRight = new TalonSRX(-2);

    TalonSRXMotorLeftSlave = new TalonSRX(-3);
    TalonSRXMotorRightSlave = new TalonSRX(-4);

    
  }

  @Override
  public void teleopPeriodic() {
    leftJoyValue = Controller.getRawAxis(0);
    rightJoyValue = Controller.getRawAxis(1);

    motorPowerLeft = -rightJoyValue * powerMultiplier + leftJoyValue * powerMultiplier;

    motorPowerRight =  -rightJoyValue * powerMultiplier - leftJoyValue * powerMultiplier;

    TalonSRXMotorLeft.set(ControlMode.PercentOutput, motorPowerLeft); #moves according to left joystick
    TalonSRXMotorRight.set(ControlMode.PercentOutput, motorPowerRight); #moves according to right joystick

    TalonSRXMotorLeftSlave.follow(TalonSRXMotorLeft); #rest of the motors follow along
    TalonSRXMotorRightSlave.follow(TalonSRXMotorRight);
  }

}
```
## 4. Further Reading
- https://github.com/iron-claw-972/HowToProgramming
- https://www.ctr-electronics.com/downloads/pdf/Legacy%20Talon%20SRX%20Software%20Reference%20Manual.pdf
- https://www.revrobotics.com/content/sw/max/sw-docs/java/com/revrobotics/CANSparkMax.html
- http://www.ctr-electronics.com/downloads/api/java/html/classcom_1_1ctre_1_1phoenix_1_1motorcontrol_1_1can_1_1_talon_s_r_x.html
