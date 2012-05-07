Intro
====================

Document explaining process of programming TI F28335 eZdsp using Simulink models




Basic configuration
-------------------

Steps to create Simulink model which can be used to generate DSP program are simple:

1. Create new Simulink model
1. Drop new _Simulink Coder/Target Preferences_ block on the model. In _Initializa Configuration Parameters_ select:
	* IDE/Tool Chain: _Texas Instruments Code Composer Studio_
	* Board: _SD F28355 eZdsp_Do you want to update the model's Configuration Parameters to correspond to your selections: _Yes_
1. When in your model window, open _Tools/Code Generation/Options..._ dialog. In _Code Generation/IDE Link_ select appropriate _Build action_:
	* _Build_ - if you just want to generate and compile code, or
	* _Build_and_execute_ - if you want to generate code, compile it and then automatically run on the DSP

**You can find preconfigured model file in [models/empty.mdl](models/empty.mdl).**

Now it should be possible to execute _Tools/Code Generation/Build Model_ action (_Ctrl+B_) (program doesn't do anything useful at this point, though).

Interrupt service routines
--------------------------

Our program is interrupt-based - pieces of code are execute whenever interrupt occures. For that we need to place _Embedded Coder/C280x/C28x3x Hardware Interrupt_ and _Simulink/Function-Call Subsystem_ blocks in the model. Then we connect interrupt output to function-call function() trigger input and set interrupt block configuration (only one interrupt for now):

* CPU interrupt numbers: _[3]_
* PIE interrupt numbers: _[1]_
* Simulink task priorities: _[30]_
* Preemption flags: _[0]_

This configuration triggers function execution whenever _EPWM1_ interrupt occures.

**Important: You have to have _ePWM1_ block in the model and you need it to have interrupts enabled (_Block Parameters/Event Trigger/Enable ePWM interrupt_).**

Examples
--------

* [models/dcmotor_controller.mdl](models/dcmotor_controller.mdl) - rotates DC motor using hardcoded angular speed value
	* **TODO: add information about motor model, PWM and interrupts used**
