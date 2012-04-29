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

**You can find preconfigured model file in `models/empty.mdl`.**

Now it should be possible to execute _Tools/Code Generation/Build Model_ action (_Ctrl+B_) (program doesn't do anything useful at this point, though).

Examples
--------

* `models/cdmotor_controller.mdl` - rotates DC motor using hardcoded angular speed value
	* **TODO: add information about motor model, PWM and interrupts used**
