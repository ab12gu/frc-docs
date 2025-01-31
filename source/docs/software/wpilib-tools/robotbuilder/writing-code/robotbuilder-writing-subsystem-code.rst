Writing the Code for a Subsystem
================================

Adding code to create an actual working subsystem is very straightforward. For simple subsystems that don't use feedback it turns out to be extremely simple. In this section we will look at an example of a `Claw` subsystem. The `Claw` subsystem also has a limit switch to determine if an object is in the grip.

RobotBuilder Representation of the Claw Subsystem
-------------------------------------------------

.. image:: images/writing-subsystem-code-1.png

The claw at the end of a robot arm is a subsystem operated by a single VictorSPX Motor Controller. There are three things we want the motor to do, start opening, start closing, and stop moving. This is the responsibility of the subsystem. The timing for opening and closing will be handled by a command later in this tutorial. We will also define a method to get if the claw is gripping an object.

Adding Subsystem Capabilities
-----------------------------

.. tab-set::

   .. tab-item:: Java
      :sync: java

      .. code-block:: java
        :linenos:
        :lineno-start: 11
        :emphasize-lines: 63-77

        // ROBOTBUILDER TYPE: Subsystem.

        package frc.robot.subsystems;


        import frc.robot.commands.*;
        import edu.wpi.first.wpilibj.livewindow.LiveWindow;
        import edu.wpi.first.wpilibj2.command.SubsystemBase;

        // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=IMPORTS
        import edu.wpi.first.wpilibj.DigitalInput;
        import edu.wpi.first.wpilibj.motorcontrol.MotorController;
        import edu.wpi.first.wpilibj.motorcontrol.PWMVictorSPX;

            // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=IMPORTS


        /**
         *
         */
        public class Claw extends SubsystemBase {
            // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CONSTANTS
        public static final double PlaceDistance = 0.1;
        public static final double BackAwayDistance = 0.6;

            // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CONSTANTS

            // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS
        private PWMVictorSPX motor;
        private DigitalInput limitswitch;

            // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS

            /**
            *
            */
            public Claw() {
                // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CONSTRUCTORS
        motor = new PWMVictorSPX(4);
         addChild("motor",motor);
         motor.setInverted(false);

        limitswitch = new DigitalInput(4);
         addChild("limit switch", limitswitch);



            // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CONSTRUCTORS
            }

            @Override
            public void periodic() {
                // This method will be called once per scheduler run

            }

            @Override
            public void simulationPeriodic() {
                // This method will be called once per scheduler run when in simulation

            }

            public void open() {
                motor.set(1.0);
            }

            public void close() {
                motor.set(-1.0);
            }

            public void stop() {
                motor.set(0.0);
            }

            public boolean isGripping() {
                return limitswitch.get();
            }

        }

   .. tab-item:: C++
      :sync: cpp

      .. code-block:: cpp
         :linenos:
         :lineno-start: 11
         :emphasize-lines: 38-52

         // ROBOTBUILDER TYPE: Subsystem.

         // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=INCLUDES
         #include "subsystems/Claw.h"
         #include <frc/smartdashboard/SmartDashboard.h>

         // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=INCLUDES

         Claw::Claw(){
             SetName("Claw");
             // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS
             SetSubsystem("Claw");

          AddChild("limit switch", &m_limitswitch);


          AddChild("motor", &m_motor);
          m_motor.SetInverted(false);

             // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS
         }

         void Claw::Periodic() {
             // Put code here to be run every loop

         }

         void Claw::SimulationPeriodic() {
             // This method will be called once per scheduler run when in simulation

         }

         // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CMDPIDGETTERS

         // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CMDPIDGETTERS


         void Claw::Open() {
             m_motor.Set(1.0);
         }

         void Claw::Close() {
             m_motor.Set(-1.0);
         }

         void Claw::Stop() {
             m_motor.Set(0.0);
         }

         bool Claw::IsGripping() {
             return m_limitswitch.Get();
         }

Add methods to the ``claw.java`` or ``claw.cpp`` that will open, close, and stop the claw from moving and get the claw limit switch. Those will be used by commands that actually operate the claw.

.. note:: The comments have been removed from this file to make it easier to see the changes for this document.

Notice that member variable called ``motor`` and ``limitswitch`` are created by RobotBuilder so it can be used throughout the subsystem. Each of your dragged-in palette items will have a member variable with the name given in RobotBuilder.

Adding the Method Declarations to the Header File (C++ Only)
------------------------------------------------------------

.. tab-set::

   .. tab-item:: C++
      :sync: cpp

      .. code-block:: cpp
         :linenos:
         :lineno-start: 11
         :emphasize-lines: 30-33

         // ROBOTBUILDER TYPE: Subsystem.
         #pragma once

         // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=INCLUDES
         #include <frc2/command/SubsystemBase.h>
         #include <frc/DigitalInput.h>
         #include <frc/motorcontrol/PWMVictorSPX.h>

         // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=INCLUDES

         /**
          *
          *
          * @author ExampleAuthor
          */
         class Claw: public frc2::SubsystemBase {
         private:
             // It's desirable that everything possible is private except
             // for methods that implement subsystem capabilities
             // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS
         frc::DigitalInput m_limitswitch{4};
         frc::PWMVictorSPX m_motor{4};

             // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=DECLARATIONS
         public:
         Claw();

             void Periodic() override;
             void SimulationPeriodic() override;
             void Open();
             void Close();
             void Stop();
             bool IsGripping();
             // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CMDPIDGETTERS

             // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CMDPIDGETTERS
             // BEGIN AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CONSTANTS
         static constexpr const double PlaceDistance = 0.1;
         static constexpr const double BackAwayDistance = 0.6;

             // END AUTOGENERATED CODE, SOURCE=ROBOTBUILDER ID=CONSTANTS


         };

In addition to adding the methods to the class implementation file, ``Claw.cpp``, the declarations for the methods need to be added to the header file, ``Claw.h``. Those declarations that must be added are shown here.

To add the behavior to the claw subsystem to handle opening and closing you need to :doc:`define commands <../introduction/robotbuilder-creating-command>`.
