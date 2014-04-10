#PRISM Simulation
##Design
###ALU Analysis
#####ALU Modifications: 
The first thing I did in the ALU_shell.vhd file was try and understand exactly what it did. I saw that there were 4 signals associated with it, namely _OpSel_, _Accumulator_, _Data_, and _Result_. I then referenced the ALU schematic to try and put a place on the picture to each of these signals. I then proceeded further down the ALU_shell file and under the ALU architecture section, and under the process statement, I added my if/elsif statements that would determine the value of _Result_. I had eight if/elsif statements, one for each value of _OpSel_ and likewise one for each operation that required implementation. in general, my code looked like the code found below:
    if OpSel="010" then
      Result <= "implementation of operation";
    elseif OpSel="011" then
      ...
Below is a picture with the items colored RED indicating what I added.

![Schematic](https://..... "What I added")

#####Test and Debug
Once I had implemented each operation, I checked my syntax and proceeded to concern myself with the errors being thrown in the ALU_testbench.vhd file. One such error that held me up for a bit was a missing semi-colon in the Component declaration of the ALU (on line 50 of the template, after END COMPONENT). I was also having trouble due to the fact that the signal declarations below the component statement were named the same thing as the signals in the Port Map of the ALU. I have named my signals the same in previous testbenches, and could not figure out why it wouldn't work this time, so I simply added a "Sig" to the end of each of the signals declared below the Component statement and the change worked. I checked my syntax and proceeded with the Simulate Behavioral Model function. 
#####More Testing and Debugging
During my first test of my ALU design, I noticed that the results were not matching what I had anticipated. For operations like NEG and NOT, my _Result_ output was basing itself on the current value of _Data_ and not on the value of _Acculumator_. I referenced the PRISM manual word document to verify what each operation is supposed to do - a silly thing to do after the fact, I realize that now, - and found that the problem was that I had simply misunderstood that these operations were supposed to be performed on _Acculumator_ and not on _Data_. It was easy to make the few changes in my ALU_shell file, switching what had been _Data_ with _Accumulator_, re-check my syntax, and re-launch the Simulate Behavior Model function. 



Once I had gone through this process for each operation, and verified that each one worked properly, I took a screenshot of my simulation results, which can be found below. 

![waveform](https://github.com/JasonPluger/PRISM/blob/master/ALU_testbench_waveform.JPG "ALU simulation waveform")






##Reverse Engineering
#####Simulation Analysis - ALU Testbench, How I knew it was correct
Once I was sure NEG and NOT were correct, I proceeded to check each operation by first going through each one on paper by hand, and then verifying the answer I got on the paper with the one generated by the simulation. For example, for the ROR operation, I looked at the _OpSel_ input and made sure that it matched with what ROR is supposed to be (3 in this case, or 011 in binary). I then looked at the description in the PRISM manual to ensure I was doing the operation to the correct specification laid out in it. Then, I looked at the current value of _Acculumator_ and rotated the bits 1 spot to the right, and then looked at the value of _Result_ to verify that it matched the value I had on paper. I did the same for however many different inputs there were for that operation, and proceeded to do the same for each individual operation - verifying the _OpSel_ input with the correct operation, doing the operation on paper with the values of _Data_ and/or _Accumulator_, and verifying my on-paper answer with the value of _Result_ from the simulation. 

#####Datapath Simulation Analysis
######50-100ns
For the Reverse Engineering of the 50-100ns region of my simulation, I referenced the waveform below.

![50-100 Waveform](https://github.com/JasonPluger/PRISM/blob/master/Datapath_testbench_waveform_50-100.JPG "50-100 Waveform")

a. At 50ns, Data contains a 3 which the controller placed on the Databus from the Program Counter at 46ns. 
b. At 55ns, the controller places whatever is on the Databus, a 3 in this case, into the Instruction Register becuause the clock is on a rising edge, and IRLd is high. 3 is the OpCode for ROR.
c. At 75ns, the ROR function is executed by the controller because AccLd is high AND we have a rising clock edge. The values in the Accumulator before and after execution are Bh and Dh respectively, which is correct. 
d) At 75ns the Program Counter is now at a value of 4 and the Controller places this value on the Databus. 
e) at 85ns the Instruction Register is given the value on the Databus, 4, which is the OpCode corresponding to OR operation. 
f) Between 85ns and 100ns, the value of Accumulator does not change.




Dr. Neebel checked my demonstration of ALU_testbench on 8Apr2014.
Dr. Neebel checked my demonstration of Datapath_testbench on 10Apr2014.

Documentation: 7Apr14: EI with Dr. Neebel; we discussed whether I had to create separate entities for some operations, and he told me it was acceptable to do so, but not necessary. We also discussed the various ways of implementing the architechture of the ALU_shell file - in particular using a process statement with either case-statements or if/elsif statements. Is the process statement required when using the if/elsif? 
