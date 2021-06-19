# Writing Basic Robot Drive Code

[Press here to return to main page.](https://github.com/iron-claw-972/Curriculum2020)

## 1. Purpose

This activity will introduce several key topics, such as how to spin a motor, how to read a joystick, and how to write basic robot drive code. In addition, it will describe how to use some of the features of FRC VS Code, such as creating a robot project with the WPILib extension. It will also introduce you to command based programming.

## 2. Materials

- FRC VS Code
  - Downloading FRC VS Code is detailed [here](https://github.com/iron-claw-972/Curriculum2020/blob/master/InstallingFrcPrereqs.md#frc-vscode).
- Github Desktop

## 3. Instructions

# Creating a Project

1. Refer to the Github tutorial and create a new repository. The default location should be in Documents/Github/YourRepo
2. Next open up VSCode 2021. 
3. Click the W icon in the top right corner. This should open up a text box. Type in "Create a new project" and press enter. This should bring you to the WPILib New Project Creator
4. Click "Select a project type (Example or Template)" 
5. Press template, then Java, then Command Robot (not Old Command Robot)
6. Next click "Select a new project folder" and find and select the repository folder you created previously.
7. Enter a valid project name using capital letters to separate words with no spaces (ex. "ExampleProject")
8. Enter your team number
9. Click generate project
10. Inside github desktop, many files should have appeared. Write a summary like "Created command-based project" and click commit to master.


![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/commitcreatechanges.png)


# Making robot move

1. First we will need to install some vendor libraries. Install the ones for CTRE and Rev, which will allow us to control CTRE and Rev motor controllers, like the TalonSRX or SparkMax. Installation is detailed [here](https://github.com/iron-claw-972/Curriculum2020/blob/master/InstallingFrcPrereqs.md#vendor-libraries). You must have an internet connection to install the vendor libraries (you cannot be connected to the robot, must be connected to wifi).
2. Next navigate to java\frc\robot by clicking the arrows in the folder structure on the left:

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/clickrobotdotjava.png)

3. You should see Main.java, Robot.java, RobotContainer.java, and Constants.java. You should also see 2 folders: commands and subsystems. The template has already filled out Main.java and Robot.java, you will not need to touch these. A brief overview of the important files we will be using:
- `RobotContainer.java`: this is the heart of your program. This class represents your robot, and contains instance variables for each of the subsystems (like drivetrain, elevator, intake, shooter, etc.). It is also the place where commands are bound to triggers. Commands are objects that tell subsystems to do something (like DriveForward or IntakeBall), and triggers are things that trigger commands (like a joystick button press, button release, or sensor measurement from the robot)
- `Subsystems`: Subsystems are classes that represent mechanisms on the robot, and subsystem classes have methods that tell mechanisms to do something. For example, you might have a subsystem for your ball shooter mechanism, with methods like SpinUpToLowSpeed, SpinUpToHighSpeed, StopSpinning, ReverseDirection, and ShootBalls. All subsystems shoul be placed in the subsystems folder.
- `Constants.java`: This class contains variables that represent constants about the robot. For example, it could hold IDs of motors, pneumatics, and more. It could also store gear ratios, measurements, or other properties. Subsystems would then use the constants stored in Constants.java
- `Commands`: Commands tell your robot to do something. These classes are instantiated with the subsystems they use in their contructor, and have 3 main methods. The initialize method is called when the command is triggered, the execute method is called around 50 times a second when the command is scheduled, and end method is called when the command is done. 



4. Since we want to make our robot drive, we will first have to create a drivetrain subsystem. To do this, create a file called Drivetrain.java in the subsystems folder (right click on subsystems and click "New file"). Inside of it, you can paste the basic template for a subsystem.:

```
package frc.robot.subsystems;

import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class Drivetrain extends SubsystemBase {
  /**
   * Creates a new Drivetrain.
   */
  public Drivetrain() {

  }

  @Override
  public void periodic() {
    // This method will be called once per scheduler run
  }
}

```


5. Right now, this subsystem doesn't make the robot do anything. In FRC, there are mainly 4 types of motor controllers we use - the VictorSPX, the TalonSRX, the TalonFX, and the SparkMax. Each of these has a different object. However, we can use WPI's SpeedControllerGroup object to make them work the same way. First let's go over the basics ofeach motor controller.

- TalonSRX: To initialize a motor controller, we need the CAN Device ID the motor controller is plugged in to. This can be found on the robot, or with the Phoenix Tuner application. When you don't know what the id will be, you can just leave it at -1 (non-existant id) so people know that the CAN IDs need to be added. Add `TalonSRXMotor = new TalonSRX(-1);` to robotInit. To spin the motor, add `TalonSRXMotor.set(ControlMode.PercentOutput, .9);`. This will tell the robot to keep spinning the motor at 90% power. Using -0.9 will cause the motor to spin the other way. Here are some examples. A special feature of the Talon SRX motor controller is that it has port for an encoder to plug into it (the VictorSPX does not have this).:

```
//to import
import com.ctre.phoenix.motorcontrol.ControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonSRX;

//to create
TalonSRX TalonSRXMotor;

//to instantiate
TalonSRXMotor = new TalonSRX(-1);

//to set
TalonSRXMotor.set(ControlMode.PercentOutput, .9);

```
- VictorSPX: The VictorSPX works in the same way as the TalonSRX except it a different class and cannot accomodate an encoder connected to it.
```
//to import
import com.ctre.phoenix.motorcontrol.ControlMode;
import com.ctre.phoenix.motorcontrol.can.VictorSPX;

//to create
VictorSPX VictorSPXMotor;

//to instantiate
VictorSPXMotor = new VictorSPX(-1);

//to set
VictorSPXMotor.set(ControlMode.PercentOutput, .9);
```
- TalonFX: The TalonFX also works similar to the TalonSRX except you should use the TalonFXControlMode class. The TalonFX is the built in motor controller for the Falcon500 motor, and comes with an encoder connected to it.:
```
//to import
import com.ctre.phoenix.motorcontrol.TalonFXControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonFX;

//to create
TalonFX TalonFXMotor;

//to instantiate
TalonFXMotor = new TalonFX(-1);

//to set
TalonFXMotor.set(TalonFXControlMode.PercentOutput, .9);
```

- The SparkMax uses the CANSparkMax class as shown below. If you are using the SparkMax with a NEO motor, you MUST plug in an encoder wire as it is brushless. Otherwise the motor will fry. If you are not using a brushless motor make sure to use `MotorType.kBrushed` instead of `MotorType.kBrushless`. To check if a SparkMax is driving a brushed or brushless motor, check how many wires are coming out of it (excluding the encoder wires). There will be three fat wires for brushless motors and two fat wires for brushed motors.

```
//to import
import com.revrobotics.CANSparkMax;
import com.revrobotics.CANSparkMaxLowLevel.MotorType;

//to create
CANSparkMax SparkMaxMotor;

//to instantiate
SparkMaxMotor = new CANSparkMax(-1, MotorType.kBrushless);

//to set
SparkMaxMotor.set(.9);
```

6. Now we know how to spin motors, so we can finish creating our drivetrain subsystem. We will be creating a drivetrain subsystem for hypothetical 6 wheeled west coast drive robot that has 2 Falcon 500 motors per side (which have built in TalonFX motor controllers). Below is the subsystem filled in with the TalonFX objects and imports. Notice that we are getting our motorIDs from the constants class (you can add static final int variables in Constants.java).

```
package frc.robot.subsystems;
import frc.robot.Constants;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import com.ctre.phoenix.motorcontrol.TalonFXControlMode;
import com.ctre.phoenix.motorcontrol.can.*;


public class Drivetrain extends SubsystemBase {
  private final TalonFX leftMotor1 = new TalonFX(Constants.leftMotor1ID);
  private final TalonFX leftMotor2 = new TalonFX(Constants.leftMotor2ID);
  private final TalonFX rightMotor1 = new TalonFX(Constants.rightMotor1ID);
  private final TalonFX rightMotor2 = new TalonFX(Constants.rightMotor1ID);

  public Drivetrain() {

  }

  @Override
  public void periodic() {
    // This method will be called once per scheduler run
  }
}

```
Since our robot is a differential drive robot (which means there are two sides that are controlled independently), we can first group our motor controllers into SpeedControllerGroups for the left and right sides of the robot as shown below. SpeedControllerGroups behave similar to motor controllers. For example, you can use .set(0.9) on them to set all of the motors in the group to 90% power. Additionally, we must invert one side of the drivetrain. If we did not invert one side, and we set both the left and right motors to positive 90% motor power, it would actually spin in an circle since the left and right motors are rotated from each other. Since we would like it to go forward when we set the left and right motor groups to a positive speed, we must invert one side. As you can see below, this is done in the contructor.
```
package frc.robot.subsystems;
import frc.robot.Constants;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import edu.wpi.first.wpilibj.SpeedControllerGroup;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import com.ctre.phoenix.motorcontrol.TalonFXControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonFX;


public class Drivetrain extends SubsystemBase {
  private final TalonFX leftMotor1 = new TalonFX(Constants.leftMotor1ID);
  private final TalonFX leftMotor2 = new TalonFX(Constants.leftMotor2ID);
  private final TalonFX rightMotor1 = new TalonFX(Constants.rightMotor1ID);
  private final TalonFX rightMotor2 = new TalonFX(Constants.rightMotor1ID);

  private final SpeedControllerGroup leftMotors = 
        new SpeedControllerGroup(leftMotor1, leftMotor2);

  private final SpeedControllerGroup rightMotors = 
        new SpeedControllerGroup(rightMotor1, rightMotor2);


  public Drivetrain() {
    rightMotors.setInverted(true);
  }

  @Override
  public void periodic() {
    // This method will be called once per scheduler run
  }
}

```
Now that we have our SpeedControllerGroups, let's create an arcade drive functionality for our subsystem (arcade drive is where one joystick indicates the left-right turning amount, and the other joystick indicates the forward-backwards throttle power). To do this, we will create 2 instance variables that store the power that is given to the left and right sides of the robot, and create an arcade drive method that sets them depending on the throttle and turning values that are inputed. Then, in the periodic method, we will set the motor power to the actual motors.

```
package frc.robot.subsystems;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import edu.wpi.first.wpilibj.SpeedControllerGroup;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import com.ctre.phoenix.motorcontrol.TalonFXControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonFX;


public class Drivetrain extends SubsystemBase {
  private final TalonFX leftMotor1 = new TalonFX(Constants.leftMotor1ID);
  private final TalonFX leftMotor2 = new TalonFX(Constants.leftMotor2ID);
  private final TalonFX rightMotor1 = new TalonFX(Constants.rightMotor1ID);
  private final TalonFX rightMotor2 = new TalonFX(Constants.rightMotor1ID);

  private final SpeedControllerGroup leftMotors = 
        new SpeedControllerGroup(leftMotor1, leftMotor2);

  private final SpeedControllerGroup rightMotors = 
        new SpeedControllerGroup(rightMotor1, rightMotor2);

  private double leftPower = 0;
  private double rightPower = 0;

  public Drivetrain() {
    rightMotors.setInverted(true);
  }

  public void arcadeDrive(double throttle, double turn){
    //turn is positive=>left motor increases=>turns right
    leftPower = throttle + turn; 
    rightPower = throttle - turn;
  }

  @Override
  public void periodic() {
    leftMotors.set(leftPower);
    rightMotors.set(rightPower);
  }
}

```




6. Great, we know how to spin many types of motors. But how do we get this code onto the robot and run it? First we need to connect our computer to the robot, either with a USB cable, ethernet cable, or through wifi. When using wifi, check the name of the network of the robot (it will be on the bridge), and connect to the wifi network. Then in FRC VSCode (after saving your code), click the W in the upper right hand corner, and select deploy robot code. If the deploy is not successful, it could be due to your code not compiling, not having the correct vendor libraries, not being connected to the robot, or another reason. Make sure to read the error message. If code code deploys successfully, you can open FRC Driver Station, shout ENABLING, and then click enable. If the robot does something unexpected and is not suspended on blocks, you can press disable or the space key to stop it. Note that if you press the space key you will not be able to enable it again until you redeploy.

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
