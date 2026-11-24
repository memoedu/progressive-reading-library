## Iverilog

```Bash
# vlog
iverilog -t null

# 不支持 svlog

# 不支持 vhdl
```

正常输出：

```Go
无
```

错误输出：

```Plain
library/Apply/Comm/FDE/AGC/AGC.v:15: syntax error
I give up.
```



## Vivado

```Bash
# vlog
xvlog file.v --nolog

# svlog
xvlog file.v --sv --nolog

# vhdl
xvhdl file.v --nolog
```

正常输出：

```Plain
INFO: [VRFC 10-2263] Analyzing Verilog file "/home/dide/project/Digital-IDE/library/Apply/Comm/FDE/AGC/AGC.v" into library work
INFO: [VRFC 10-311] analyzing module AGC
```

错误输出：

```Plain
INFO: [VRFC 10-2263] Analyzing Verilog file "/home/dide/project/Digital-IDE/library/Apply/Comm/FDE/AGC/AGC.v" into library work
INFO: [VRFC 10-311] analyzing module AGC
ERROR: [VRFC 10-4982] syntax error near 'reg' [/home/dide/project/Digital-IDE/library/Apply/Comm/FDE/AGC/AGC.v:15]
ERROR: [VRFC 10-8549] Verilog 2000 keyword 'reg' used in incorrect context [/home/dide/project/Digital-IDE/library/Apply/Comm/FDE/AGC/AGC.v:15]
```



## Modelsim

```Bash
# vlog
vlog file.v -quiet -nologo -work ~/.digital-ide

# svlog
vlog file.v -quiet -nologo -sv -work ~/.digital-ide

# vhdl
vcom file.v -quiet -nologo -2008 -work ~/.digital-ide
```

正常输出：

```
无 
```

错误输出：

```
** Error: (vlog-13069) library/Apply/Comm/FDE/AGC/AGC.v(15): near "reg": syntax error, unexpected reg, expecting ';'.
```





## Verilator

```Bash
# vlog
verilator file.v --lint-only -Wall

# svlog
verilator file.v --lint-only -sv -Wall

# 不支持 vhdl
```

正常信息：

```
无
```

警告信息：

```
%Warning-IMPLICIT: library/Apply/Comm/FDE/AGC/AGC.v:23:12: Signal definition not found, creating implicitly: 'r_rms'
                                                         : ... Suggested alternative: 'ref_rms'
   23 |     assign r_rms     = (reference * reference);
      |            ^~~~~
                   ... Use "/* verilator lint_off IMPLICIT */" and lint_on around source to disable this message.
%Warning-WIDTH: library/Apply/Comm/FDE/AGC/AGC.v:23:22: Operator ASSIGNW expects 1 bits on the Assign RHS, but Assign RHS's MUL generates 16 bits.
                                                      : ... In instance AGC
   23 |     assign r_rms     = (reference * reference);
      |                      ^
%Warning-WIDTH: library/Apply/Comm/FDE/AGC/AGC.v:28:38: Operator SUB expects 32 bits on the LHS, but LHS's VARREF 'r_rms' generates 1 bits.
                                                      : ... In instance AGC
   28 |     assign tmp_level = $signed(r_rms - x_mod) >>> 1;
      |                                      ^
%Warning-WIDTH: library/Apply/Comm/FDE/AGC/AGC.v:40:18: Operator ASSIGNW expects 16 bits on the Assign RHS, but Assign RHS's SHIFTRS generates 32 bits.
                                                      : ... In instance AGC
   40 |     assign y_out = (gain * x_in) >>> DWIDTH;
      |                  ^
%Warning-SYMRSVDWORD: library/Apply/Comm/FDE/AGC/AGC.v:9:41: Symbol matches C++ common word: 'reference'
    9 |         input  wire        [DWIDTH-1:0] reference,
      |                                         ^~~~~~~~~
%Error: Exiting due to 5 warning(s)
```

错误信息：

```
%Error: library/Apply/Comm/FDE/AGC/AGC.v:15:5: syntax error, unexpected reg, expecting ';'
   15 |     reg  signed [(DWIDTH*2)-1:0] gain; 
      |     ^~~
%Error: library/Apply/Comm/FDE/AGC/AGC.v:23:5: syntax error, unexpected assign
   23 |     assign r_rms     = (reference * reference);
      |     ^~~~~~
%Error: Exiting due to 2 error(s)
```





## Verible

```bash
# vlog
verible-verilog-syntax file.v --lint-only -Wall

# svlog
verible-verilog-syntax file.v --lint-only -sv -Wall

# 不支持 vhdl
```



正常输出：

```
无
```

错误输出：

```
library/Apply/Comm/FDE/AGC/AGC.v:15:5-7: syntax error at token "reg"
library/Apply/Comm/FDE/AGC/AGC.v:23:5-10: syntax error at token "assign"
library/Apply/Comm/FDE/AGC/AGC.v:24:5-10: syntax error at token "assign"
library/Apply/Comm/FDE/AGC/AGC.v:26:5-10: syntax error at token "assign"
library/Apply/Comm/FDE/AGC/AGC.v:28:5-10: syntax error at token "assign"
library/Apply/Comm/FDE/AGC/AGC.v:29:5-10: syntax error at token "assign"
library/Apply/Comm/FDE/AGC/AGC.v:31:5-10: syntax error at token "always"
library/Apply/Comm/FDE/AGC/AGC.v:34:9-11: syntax error at token "end"
library/Apply/Comm/FDE/AGC/AGC.v:37:9-11: syntax error at token "end"
library/Apply/Comm/FDE/AGC/AGC.v:42:1-9: syntax error at token "endmodule"
```



