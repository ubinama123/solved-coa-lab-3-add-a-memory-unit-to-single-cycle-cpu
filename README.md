Download Link: https://assignmentchef.com/product/solved-coa-lab-3-add-a-memory-unit-to-single-cycle-cpu
<br>
Based on Lab 2 (simple single-cycle CPU), add a memory unit to implement a complete single-cycle CPU which can run R-type, I-type and jump instructions.

2. Requirement

<ul>

 <li>Please use Icarus Verilog and GTKWave as your HDL simulator.</li>

 <li>Please attach your names and student IDs as comment at the top of each file.</li>

 <li>Top module’s name and IO port reference Lab2.</li>

</ul>

Reg_File[29] represents stack point. Initialize Reg_file[29] to 128 while others to

You may add control signals to Decoder, e.g.

<ul>

 <li>Branch_o</li>

 <li>Jump_o</li>

 <li>MemRead_o</li>

 <li>MemWrite_o</li>

 <li>MemtoReg_o</li>

</ul>

<h1>3. Requirement description</h1>

<strong>Lw instruction  </strong>memwrite is 0, memread is 1, regwrite is 1

Reg[rt] ← Mem[rs+imm]




<strong>Sw instruction </strong>memwrite is 1, memread is 0

Mem[rs+imm] ← Reg[rt]




<strong>Branch instruction </strong>branch is 1，and decide branch or not by do AND with the zero signal from ALU.

PC = PC + 4 + (sign_Imm &lt;&lt;2)

<strong>Jump instruction </strong>jump is 1

PC = {PC[31:28], address&lt;&lt;2}




<h1>NOP instruction</h1>

No operation – do nothing




<ol start="4">

 <li><strong>Code (80 pts.) </strong></li>

</ol>

<strong>(1)</strong> <strong>Basic Instruction: (50 pts.) </strong>

<strong>Lab2 instruction + mul, lw, sw, j, NOP </strong>

<strong> </strong>

<h1>R-type</h1>

<table width="513">

 <tbody>

  <tr>

   <td width="83">Op[31:26]</td>

   <td width="84">Rs[25:21]-</td>

   <td width="84">Rt[20:16]</td>

   <td width="85">Rd[15:11]</td>

   <td width="94">Shamt[10:6]</td>

   <td width="83">Func[5:0]</td>

  </tr>

  <tr>

   <td width="83"> <strong>I-type </strong></td>

   <td width="84"> </td>

   <td width="84"> </td>

   <td colspan="3" width="262"> </td>

  </tr>

  <tr>

   <td width="83">Op[31:26]</td>

   <td width="84">Rs[25:21]-</td>

   <td width="84">Rt[20:16]</td>

   <td colspan="3" width="262">Immediate[15:0]</td>

  </tr>

  <tr>

   <td width="83"> <strong>Jump </strong></td>

   <td width="84"> </td>

   <td width="84"> </td>

   <td colspan="3" width="262"> </td>

  </tr>

  <tr>

   <td width="83">Op[31:26]</td>

   <td width="84"> </td>

   <td width="84"> </td>

   <td colspan="3" width="262">Address[25:0]</td>

  </tr>

 </tbody>

</table>




<table width="523">

 <tbody>

  <tr>

   <td width="105"><strong>instruction </strong></td>

   <td width="99"><strong>op[31:26] </strong></td>

   <td width="99"> </td>

   <td width="99"> </td>

   <td width="122"> </td>

  </tr>

  <tr>

   <td width="105">lw</td>

   <td width="99">6’b100011</td>

   <td width="99">Rs[25:21]</td>

   <td width="99">Rt[20:16]</td>

   <td width="122">Immediate[15:0]</td>

  </tr>

  <tr>

   <td width="105">sw</td>

   <td width="99">6’b101011</td>

   <td width="99">Rs[25:21]</td>

   <td width="99">Rt[20:16]</td>

   <td width="122">Immediate[15:0]</td>

  </tr>

  <tr>

   <td width="105">j</td>

   <td width="99">6’b000010</td>

   <td width="99"> </td>

   <td colspan="2" width="221">Address[25:0]</td>

  </tr>

 </tbody>

</table>




<h1>Mul is R-type instruction</h1>

<table width="506">

 <tbody>

  <tr>

   <td width="83">0</td>

   <td width="84">Rs[25:21]-</td>

   <td width="84">Rt[20:16]</td>

   <td width="85">Rd[15:11]</td>

   <td width="87">0</td>

   <td width="84">6’b011000</td>

  </tr>

 </tbody>

</table>

<strong> </strong>

<h1>NOP instruction</h1>

32 bits 0.




<strong>(2)</strong> <strong>Advance set 1: (10 pts.) </strong>

<table width="529">

 <tbody>

  <tr>

   <td width="89"><strong>instruction </strong></td>

   <td width="84"><strong>op </strong></td>

   <td width="66"><strong>rs </strong></td>

   <td width="66"><strong>rt </strong></td>

   <td width="67"><strong>rd </strong></td>

   <td width="73"><strong>shamt </strong></td>

   <td width="84"><strong>func </strong></td>

  </tr>

  <tr>

   <td width="89">jal</td>

   <td width="84">6’b000011</td>

   <td width="66"> </td>

   <td width="66"> </td>

   <td colspan="2" width="140">Address[25:0]</td>

   <td width="84"> </td>

  </tr>

  <tr>

   <td width="89">jr</td>

   <td width="84">6’b000000</td>

   <td width="66">rs</td>

   <td width="66">0</td>

   <td width="67">0</td>

   <td width="73">0</td>

   <td width="84">6’b001000</td>

  </tr>

 </tbody>

</table>

<strong> </strong>

<h1>Jal: jump and link</h1>

In MIPS, the 31st register is used to save return address for function call.

Reg[31] saves PC+4 and address for jump




Reg[31] = PC + 4

PC = {PC[31:28], address[25:0]&lt;&lt;2}




<h1>Jr: jump to the address in the register rs</h1>

PC = Reg[rs];

e.g.：In MIPS, return could be used by jr r31 to jump to return address from JAL




Example: when CPU executes function call,




<strong>0x00000000                  Fibonacci(4); </strong>

<strong>0x00000004                  Add r0, r0, r1 </strong>

<strong> </strong>

<strong>1</strong>

<strong> </strong>

<strong> </strong><strong>3</strong>

<strong>0x00001000                  Fibonacci(){                                                                                         .                                      </strong>

<strong><sup>2</sup></strong><strong>                            .     </strong>

<strong>0x00001020                  } </strong>

<strong> </strong>

if you want to execute recursive function, you must use the stack point (Reg_File[29]).

First, store the register to memory and load back after function call has been finished. The second testbench CO_P3_test_data2.txt is the Fibonacci function. After it is done, r2 stores the final answer. Please refer to test2.txt.




<strong>(3)</strong> <strong>Advance set 2: (20 pts.) </strong>

<strong>ble</strong>(branch less equal than): if( rs &lt;= rt ) then branch

<table width="529">

 <tbody>

  <tr>

   <td width="108">6’b000110</td>

   <td width="84">rs</td>

   <td width="84">rt</td>

   <td width="253">offset</td>

  </tr>

 </tbody>

</table>




<strong>bnez</strong>(branch non equal zero): if( rs != 0 ) then branch (It is same as bne)

<table width="529">

 <tbody>

  <tr>

   <td width="108">6’b000101</td>

   <td width="84">rs</td>

   <td width="84">0</td>

   <td width="253">offset</td>

  </tr>

 </tbody>

</table>




<strong>bltz</strong>(branch less than zero): if( rs &lt; 0 ) then branch

<table width="529">

 <tbody>

  <tr>

   <td width="108">6’b000001</td>

   <td width="84">rs</td>

   <td width="84">0</td>

   <td width="253">offset</td>

  </tr>

  <tr>

   <td colspan="2" width="192"> <strong>li</strong>(load immediate) by) addi.</td>

   <td colspan="2" width="338">You don’t have to implement it, because it is similar to (and thus can be replaced</td>

  </tr>

  <tr>

   <td width="108">6’b001111</td>

   <td width="84">0</td>

   <td width="84">rt</td>

   <td width="253">immediate</td>

  </tr>

 </tbody>

</table>







<ol start="5">

 <li><strong>Testbench </strong></li>

</ol>

CO_P3_test_data1.txt tests the <strong>basic</strong> <strong>instructions</strong> (50 pts.)  CO_P3_test_data2.txt tests the <strong>advanced set 1</strong> (10 pts.).

Please refer to test1.txt and test2.txt for details. The following MIPS code is bubble sort. Please transform the MIPS code to machine code, store the machine code in CO_P3_test_data3.txt and run it (for testing advance set 2 (20 pts.)).

You don’t have to submit machine code.

<strong>  </strong>

<table width="525">

 <tbody>

  <tr>

   <td width="144"><strong>    </strong>adduaddi     addi     mul     j Jump bubble:li      limul outer:     addi     subuli  inner:     lw     lwble </td>

   <td width="121">$t0, $0, $0$t1, $0, 10$t2, $0, 13$t3, $t1, $t1$t0, 10$t1, 4$t4, $t0, $t1$t6, $0, 8$t0, $t4, $t6 $t1, 0$t2, 4($t0)$t3, 0($t0)$t2,$t3,no_swap</td>

   <td width="128"><strong>    </strong>sw     swli no_swap:addi     subu     bltzj next_turn:bnezjJump:subu Loop:addu     beqjEnd:</td>

   <td width="133">$t2, 0($t0)$t3, 4($t0) $t1, 1$t5, $0, 4$t0, $t0, $t5 $t0, next_turn inner$t1, outerEnd$t2, $t2,     $t1$t4, $t3,           $t2 $t1, $t2,             Loop bubble</td>

  </tr>

 </tbody>

</table>

<strong> </strong>

<strong> </strong>

<ol start="6">

 <li><strong>Reference architecture </strong></li>

</ol>

This lab might be used extra signal to control. Please draw the architecture you designed on