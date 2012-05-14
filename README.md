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

Our program is interrupt-based - pieces of code are execute whenever interrupt occures. For that we need to place _Embedded Coder/C280x/C28x3x Hardware Interrupt_ and _Simulink/Function-Call Subsystem_ blocks in the model. Then we connect interrupt output to function-call function() trigger input and set interrupt block configuration.

**There can be only 1 interrupt block in the model. If you want to use more than one interrupt, you have to configure needed interrupts in this interrupt block and then demux it's output.**

Interrupt list for F28335 eZdsp can be found in section _Table 6-4. PIE MUXed Peripheral Interrupt Vector Table_ in the _TMS320x2833x, 2823x System Control and Interrupts_ document.

Let's configure EPWM1_INT interrupt service routine. We need to:

* place EPWM block in the model
* set Module to _ePWM1_ and check _Enable ePWM Interrupt_ in EPWM's block properties
* set following values in interrupt's block properties:
	* CPU interrupt numbers: _[3]_ (EPWM Interrupts group)
	* PIE interrupt numbers: _[1]_ (EPWM1 interrupt)
	* Simulink task priorities: _[30]_
	* Preemption flags: _[0]_

This configuration triggers function execution whenever _EPWM1_ interrupt occures.

Examples
--------

* [models/dcmotor_controller.mdl](models/dcmotor_controller.mdl) - rotates DC motor using hardcoded angular speed value
	* **TODO: add information about motor model**
