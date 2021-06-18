# Writing Junit Tests with Mockito

[Press here to return to main page.](https://github.com/iron-claw-972/Curriculum2020)

## 1. Purpose

This activity will introduce how to write Junit Tests and how to use Mockito. These tests are a useful tool in verifying that the code works and that a given change has not broken the code.

## 2. Materials

- FRC VS Code
  - Downloading FRC VS Code is detailed [here](https://github.com/iron-claw-972/Curriculum2020/blob/master/InstallingFrcPrereqs.md#frc-vscode).

## 3. Instructions

### 3a. Modifying `build.gradle` to import libraries

Two main libraries that we will be using are:
- JUnit
- Mockito

Current FRC uses Gradle to build and deploy the robot code. Gradle is among other things, a package manager. In order to download the two previously mentioned libraries:

1. Open the `build.gradle` file, it is located in the root directory of the project.

2. Scrolling down, you should see a dependencies area:

``` java
dependencies {
    implementation wpi.deps.wpilib()
    nativeZip wpi.deps.wpilibJni(wpi.platforms.roborio)
    nativeDesktopZip wpi.deps.wpilibJni(wpi.platforms.desktop)


    implementation wpi.deps.vendor.java()
    nativeZip wpi.deps.vendor.jni(wpi.platforms.roborio)
    nativeDesktopZip wpi.deps.vendor.jni(wpi.platforms.desktop)

    testImplementation 'junit:junit:4.12'

    // Enable simulation gui support. Must check the box in vscode to enable support
    // upon debugging
    simulation wpi.deps.sim.gui(wpi.platforms.desktop, false)
}
```

3. Above the dependencies area add:

``` java
repositories {
    mavenCentral()
    jcenter()
}
```

This tells gradle where to look for the libraries when downloading them.

4. In the dependencies area add:

``` java
testCompile 'junit:junit:latest.release'
testCompile"org.mockito:mockito-core:latest.release"
```

At this point, the dependencies are should look something like this:

``` java
dependencies {
    implementation wpi.deps.wpilib()
    nativeZip wpi.deps.wpilibJni(wpi.platforms.roborio)
    nativeDesktopZip wpi.deps.wpilibJni(wpi.platforms.desktop)


    implementation wpi.deps.vendor.java()
    nativeZip wpi.deps.vendor.jni(wpi.platforms.roborio)
    nativeDesktopZip wpi.deps.vendor.jni(wpi.platforms.desktop)

    testImplementation 'junit:junit:4.12'

    testCompile 'junit:junit:latest.release'
    testCompile"org.mockito:mockito-core:latest.release"

    // Enable simulation gui support. Must check the box in vscode to enable support
    // upon debugging
    simulation wpi.deps.sim.gui(wpi.platforms.desktop, false)
}
```

5. Remove the line:

``` java
testImplementation 'junit:junit:4.12'
```

6. Build the project by pressing `ctrl-shift-p` and typing `build` before selecting the `WPILib: Build robot code` option. After the build is finished, you should be able to use JUnit and Mockito.

### 3b. Calculator Test

This following test is designed to test the Calculator.java class, which has the methods add(), multiply() and divide(). Please note that it is located in `{Project}\src\test\java\frc\robot\CalculatorTest.java`.

``` java
package frc.robot.util;

import static org.junit.Assert.*;
import org.junit.*;

public class CalculatorTest {
  public Calculator calculator = new Calculator();

  @Test
  // This function tests if the addition feature of Calculator works
  public void testAdd() {
    int actualSum = calculator.add(10, 5);

    assertEquals(10 + 5, actualSum, 1);
  }

  @Test
  // This function tests if the multiplication feature of Calculator works
  public void testMultiply() {
    int actualProduct = calculator.multiply(3, 7);

    assertEquals(3 * 7, actualProduct, 1);
  }

  @Test
  // This function makes sure that 4 / 2 does not equal 73
  public void testDivide() {
    int actualProduct = calculator.divide(4, 2);

    assertNotEquals(73, actualQuotient);
  }
}
```

This example illustrates several basic concepts in testing:
- `@Test` - Every testing function is annotated with `@Test`. If there is no annotation, it will not run when the tests are run.
- `public void` - All test functions must be `public void`.
- `assertEquals()` / `assertNotEquals()` are two ways to check if two things are equal or not equal, if you pass `assertEquals()` with parameters that are equal, the test will pass. Otherwise, the test will fail.
 - Note: The third parameter in the `assertEquals()` is the error margin.

### 3c. Mocking objects with Mockito

This is a more advanced test that makes use of Mockito. This library allows for the making of mock instances of objects. These mocks allow for the faking of functions. This is extremely useful in writing more advanced tests.

``` java
package frc.robot.controllers;

import static org.junit.Assert.*;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.doAnswer;
import static org.mockito.Mockito.mock;
import org.junit.*;
import com.revrobotics.CANSparkMax;

public class NeoDrivetrainTest
{
    // Declaration of variables used to store inputs to motors
    public double lm1Power = 0;
    public double lm2Power = 0;
    public double rm1Power = 0;
    public double rm2Power = 0;

    // Creation of mock sparks and putting them into NeoDrivetrain
    public CANSparkMax lm1 = mock(CANSparkMax.class);
    public CANSparkMax lm2 = mock(CANSparkMax.class);
    public CANSparkMax rm1 = mock(CANSparkMax.class);
    public CANSparkMax rm2 = mock(CANSparkMax.class);
    public NeoDrivetrain neoDrivetrain = new NeoDrivetrain(lm1, lm2, rm1, rm2);

    // @Before allows for the setup() method to be called before any other methods
    @Before
    public void setup() {
        // Basically replaces the .set() method for mock Spark with
        // method that puts input into variable for testing
        doAnswer(invocation -> {
            Double power = invocation.getArgument(0, Double.class);
            lm1Power = power.doubleValue();
            return null;
        }).when(lm1).set(any(Double.class));

        doAnswer(invocation -> {
            Double power = invocation.getArgument(0, Double.class);
            lm2Power = power.doubleValue();
            return null;
        }).when(lm2).set(any(Double.class));

        doAnswer(invocation -> {
            Double power = invocation.getArgument(0, Double.class);
            rm1Power = power.doubleValue();
            return null;
        }).when(rm1).set(any(Double.class));

        doAnswer(invocation -> {
            Double power = invocation.getArgument(0, Double.class);
            rm2Power = power.doubleValue();
            return null;
        }).when(rm2).set(any(Double.class));
    }

    // Simple test that calls method that calls in turn the .set function of motors
    @Test
    public void testTankDrive0() {
        neoDrivetrain.tankDrive(1.0, 1.0);

        assertEquals(1.0, lm1Power, 0.1);
        assertEquals(1.0, lm2Power, 0.1);
        assertEquals(1.0, rm1Power, 0.1);
        assertEquals(1.0, rm2Power, 0.1);
    }
}
```

As you can see, the `CANSparkMax` motor functions are mocked. In this example, the `.set()` function is mocked. This allows one to test complex systems, in this case a drivetrain class.

## 4. Further Reading

[Mockito Guide](https://javacodehouse.com/blog/mockito-tutorial/)

[Mockito Javadock](https://javadoc.io/doc/org.mockito/mockito-core/2.23.4/org/mockito/Mockito.html)
