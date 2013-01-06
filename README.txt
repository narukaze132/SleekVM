Specifications:

Sleek Virtual Machine
January 3rd-5th 2013
Version 1
Revision 5
“Ardent”
The  Sleek  Virtual  Machine  is  an  innovative
programming platform, aiming to deliver  to
application  developers  an  open  and  crossplatform alternative to proprietary solutions.
Silas NordgrenPreface
The Sleek Virtual Machine specification was written to fully document the design of an
abstract virtual machine referred to as the Sleek Virtual Machine. This abstract virtual
machine can be implemented as a software program on a computer platform, or as a
complex instruction set computing hardware processor itself, depending on the needs of
the implementer. This document was written with these implementers in mind, and does
not  refer  to  any  particular  implementation  of  the  concepts  defined  within.  An
implementation  of  the  Sleek  Virtual  Machine  must  follow  every  rule  defined  in  this
document; the phrases 'should' and 'should not' are to be treated as rules that require
inclusion and exclusion, respectively. 
The phrase 'primarily used' defines required behavior of a feature, but this feature can
be  adapted  to  include  other  functionality  as  long  as  the  functionality  defined  after
'primarily used' is satisfied.
The  phrase  'may'  marks  a  feature  not  required  in  an  implementation,  but  rather
allowed. The phrase 'implementers are encouraged' is a stronger version of may, where
the implementer isn't just allowed, but also encouraged and wanted to implementation
further functionality, and document it in a separate specification for possible inclusion
within the main specification of the Sleek Virtual Machine.
The phrase 'compliant implementation' refers to an implementation of the Sleek Virtual
Machine that follows all the requirements defined in this document. The phrase 'strictly
compliant implementation' refers to an implementation of the Sleek Virtual Machine that
includes no more and no less functionality than what is defined in this document.
1. Introduction
1.1. Sleek Virtual Machine
The Sleek Virtual Machine is an abstract computing machine, such as the Java
Virtual Machine, and has an instruction set just like any computer.  It is tied to
the  Sleek  Instruction  Set,  which  defines  the  rules  to  the  input  to  the  virtual
machine.
2. Internal Structure
The  Sleek  Virtual  Machine  has  a  few  required  internal  components  for  proper
functionality that are used by the Sleek Instruction Set. They are documented here.
2.1. Rationale
The Sleek Virtual Machine interprets a set of bytes stored in a file format. A few
internal  facilities  are  required  to  support  this  model  of  execution.  They  are
outlined below.
The phrase 'thread' refers to a thread of execution, a 
The phrase 'global' defines a component that is the same across all threads of
the virtual machine, and it's only instance is in the main thread, while the phrase
'thread-local' defines a component that has one instance for each thread running
in the virtual machine. Thread-local components have global versions in the main
thread,  however  they  are  invisible  in  spawned  threads,  who  have  their  own
thread-local components.
The  decision  to  use  registers  primarily  instead  of  a  primarily  stack-oriented
virtual machine is based on the belief that registers lead to lesser amounts of
copied  values,  which  means  more efficiency,  which  is  part  of  the  goal  of  the
Sleek Virtual Machine.
2.2. Registers
The Sleek Virtual Machine contains a set of registers manipulated by the SleekInstruction Set. These registers form the primarily variables of a program written
for the Sleek Virtual Machine. Each register holds 64 bits, but can be treated as
any 8-bit, 16-bit, 32-bit or 64-bit signed or unsigned integer, as well as a 32-bit
or 64-bit floating point value.
An implementation  of  the  Sleek  Virtual  Machine  may  choose  to  implement
further registers than those supplied here.  A table of the required registers can
be found in section 3.3 Registers. 
The registers are global components of the Sleek Virtual Machine.
2.3. Stack
The  Sleek  Virtual  Machine  contains  a  stack,  a  last-in-first-out  structure  that
primarily operates on bytes and combinations of bytes.
The stack is a thread-local component of the Sleek Virtual Machine.
2.4. Constant Table
The Sleek Virtual Machine contains the ability to define constants at runtime, as
well as at compile time through a preprocessor. The constant table is a key-value
data structure that can only take.
The constant table is  a thread-local component,  however, each instance of the
constant  table  inherit  the  key-values  found in  the  constant  table  found  in  the
main thread at the time of their creation.
3. Sleek Instruction Set
The Sleek Virtual Machine reads the programs that it executes from files, byte by byte.
These bytes form a program with the Sleek Instruction Set, which is a set of rules for
the bytes that are interpreted in the virtual machine. The instruction set is designed to
be compact and to not enforce impractical and inefficient restrictions on the file format,
however, 
3.1. Rationale
The  Sleek  Virtual  Machine  interprets  a  set  of  bytes  stored  in  a  file  format,
instead  of  actively  interpreting  source  files  in  the  language.  The  compiler
required  to  translate  between  source  files  and  the  set  of  bytes,  “bytecode”,
introduces more complexity, however, a compiler need not be implemented for
each implementation of the Sleek Virtual Machine, as bytecode produced by one
compiler  is  not  tied  to  any  platform,  unlike  implementations  of  the  virtual
machine, which require operating system-dependent calls to function. 
The compiler can provide optimizations that would not be possible to do using
pure interpretation of the source, as these optimizations cannot be realistically
performed in realtime. 
The use of  bytecode also simplifies potential ahead-of-time compilation, where
the bytecode is translated into native code prior to runtime, as well as just-intime  compilation,  where  the  bytecode  is  translated  into  native  code  during
runtime. On certain processor architectures, instructions in the Sleek Instruction
Set might match one or a few processor instructions exactly, making for optimal
execution on that platform.
3.2. Concepts
The concepts behind the Sleek Instruction Set are similar to that of most other
virtual  machine  and  processor  instruction  sets,  albeit  at  a  medium  level  ofabstraction, unlike virtual machines who often have a high level of abstraction,
and processors, who have a low level of abstraction. The Sleek Instruction Set is
designed to  offer the low-level instructions  for maximal  results  when mapping
bytecodes  to  native  processor  instructions,  while  offering  high-level  constructs
that allow for more efficiency.
3.3. Registers
The Sleek Virtual Machine contains 16 registers, that are all directly supported in
the instruction set. Quite a few of the registers serve as arguments to, or return
values from, the instructions. Each one of the registers holds 64 bits, and can be
treated as signed and unsigned integers, as well as floating point values. 
Name Byte Description
ERR 0×0 The ERR register name stands for error and is primarily used to
keep track of any errors that may have occurred in the virtual
machine, or as a result of executed instructions.
PCT 0×1 The PCT register name stands for program counter and is primarily
used to keep track of the top value of the stack.
RPT 0×2 The RPT register stands for return pointer and is primarily used to
keep track of the last value that was jumped from.
SPT 0×3 The SPT register name stands for stack pointer and is primarily
used to keep track of the top value of the stack.
CTG 0×4 The CTG register name stands for callback trigger and is primarily
used to test whether or not to execute a callback subroutine.
PTS 0×5 The PTS register name stands for pointer storage and is primarily
used to pass pointers from managed and native code.
TDF 0×6 The TDF register stands for type definition and is primarily used
to distinguish between possible representations of data sent to
instructions.
EXC 0×7 The EXC register stands for exception and is primarily used to
contain pointers to exception objects.
CNC 0×8 The CNC register name stands for constant cache and is primarily
used to store the value of temporary constants on their way to and
from the constant table.
OFF 0×9 The OFF register name stands for offset and is primarily used to
hold offsets to values.
RNG 0×A The RNG register name stands for range and is primarily used to
determine exception handling offsets.
ILE 0×B The ILE register name stands for instructions left for execution
and is primarily used for executing blocks of instructions. This
register is decremented for each executed instruction.
0×C
0×D
0×E
0×F
3.4. Standard Types
The  Sleek  Instruction  Set  has  an  amount  of  generic  instructions  that  operate
differently  depending  on  a  following  bit  that  denotes  which  type  of  data  is
incoming.  Following  below  is  a  table  of  the  bytes  that  are  used  for  these
denotations. 
Name Byte Description
SINT1 0×0 Signed integer of 1 byte.Name Byte Description
SINT2 0×1 Signed integer of 2 bytes.
SINT4 0×2 Signed integer of 4 bytes.
SINT8 0×3 Signed integer of 8 bytes.
UINT1 0×4 Unsigned integer of 1 byte.
UINT2 0×5 Unsigned integer of 2 bytes.
UINT4 0×6 Unsigned integer of 4 bytes.
UINT8 0×7 Unsigned integer of 8 bytes.
FTPV4 0×8 Floating point value of 4 bytes.
FTPV8 0×9 Floating point value of 8 bytes.
PTR48 0×A System pointer of 4 or 8 bytes.
3.5. Error Codes
The  following  is  a  table  of  error  codes.  The  amount  of  error  codes  may  be
extended in further revisions of this document.
Byte Name
0×00 UNDEFINED_ERROR
0×01 REDEFINED_CONSTANT
0×02 UNDEFINED_CONSTANT
3.6. Instructions
The  following  is  a  table  of  all  of  the  256  instructions  present  in  the  Sleek
Instruction Set. The instructions make heavy use of the internal structure of the
virtual machine. 
The instruction set is divided into categories, each allocated 16 instructions. The
first category of instructions contains instructions relating to generic instructions
made on the top value of the stack.
The phrase 'generically' or 'generic' refers to a value or functionality that differs
depending  on  the  current  value  of  the  TDF  register.  The  implementer  should
implement  a  separate  solution  for  each  of  the  possible  values  of  the  TDF
registers for each instruction.
The phrase 'immutable' refers to a value that is constant for a period of time or
permanently.
Name Byte Description
SADD 0×10 This instruction takes one argument, A. The A argument is
generically added to the top value of the stack.
SDIV 0×11 This instruction takes one argument, A. The A argument is
generically subtracted from the top value of the stack.
SMUL 0×12 This instruction takes one argument, A. The top value of the stack
is generically multiplied with the A argument.
SSUB 0×13 This instruction takes one argument, A. The top value of the stack
is generically divided by the A argument.
SINC 0×14 This instruction generically increments the top value of the stack.
SDEC 0×15 This instruction generically decrements the top value of the stack.
SSVC 0×16 This instruction takes one argument, A. The A argument is a nullterminated string of bytes. A new entry is added to the threadlocal constant table, containing the A argument and the generic top
value of the stack. If an entry with the A key already exists, thisName Byte Description
instruction sets the ERR register to the REDEFINED_CONSTANT error
value.
SLDC 0×17 This instruction takes one argument, B. The B argument is a nullterminated string of bytes. The generic result of a lookup in the
thread-local constant table is pushed to the stack. If the lookup
failed, nothing is pushed and the ERR register is set to the
UNDEFINED_CONSTANT error value.
SJMP 0×18 This instruction takes no arguments. The virtual machine sets the
RPT register to the current value of the PCT register, and the
value of the PCT register to the top value of the stack, treated as
a PTR48 value.
SREG 0×19 This instruction takes one argument, A. The virtual machine
generically sets the value of the register defined in A to the
pulled value of the stack.  
SDUP 0×1A This instruction generically pushes the current top value of the
stack to the stack.
SSWP 0×1B This instruction generically pulls twice and then pushes the pulled
values in reverse order. 
STPT 0×1C This instruction takes one argument, A. The virtual machine
increments the SPT register and generically sets the top value of
the stack to the value pointed at by the generic pointer A.
SFPT 0×1D This instruction takes one argument, A. The virtual machine sets
the value pointed at by the generic pointer A to the top value of
the stack and decrements the SPT register.
PUSH 0×1E This instruction takes one argument, A. The virtual machine
increments the SPT register and generically sets the top value of
the stack to the value of the register defined in A. 
PULL 0×1F This instruction takes one argument, A. The virtual machine
generically sets the value of the register defined in A to the top
value of the stack and decrements the SPT register.
THRW 0×20 This instruction takes one argument, A. The virtual machine sets
the bits of the EXC register to the bits of the argument A. The
virtual machine then executes the RTHW instruction.
CTCH 0×21 This instruction takes no arguments.
FNLY 0×22 This instruction takes no arguments.
FNLZ 0×23 This instruction takes one argument, A. When the virtual machine is
exiting, the virtual machine sets the PCT register to this
instruction and 
CBCK 0×24
CHCK 0×25
RTHW 0×26 This instruction takes no arguments. The virtual machine jumps to
the nearest CTCH instruction within RNG bytes and executes RNG
bytes of instructions. Then, the virtual machine jumps to the
nearest FNLY instruction within RNG bytes.
JUMP 0×27
RTRN 0×28
SUBR 0×29
JTSR 0×2A
0×2B
0×2C
0×2D
0×2E
EOIN 0×2F This byte marks the end of an instruction.