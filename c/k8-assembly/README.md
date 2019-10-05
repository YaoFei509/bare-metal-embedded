K8 Assembly

4 registers:
	r0 = 00
	r1 = 01
	r2 = 10
	pc = 11 (program counter)

256 byte program address space
256 byte memory address space

Jumps are performed by operations on the program counter (register pc)
Memory should be pre-loaded with known static data need, because there no "add immediate"
	type operations.

Use the "k8_assembler.c" to assemble *.k8 files into C header files for use with "k8_simmulator.c"
K8 assembler syntax:
	Text segment must come before data segment
	Start text segment with "text:" token, followed by legal K8 assembly operations
	Start data segment with "data:" token, followed by data specifers
	@[number] used to specify the position in memory to place data at
	Many @[number] specifiers may be used in the data segment

Also, a Notepad++ language stylesheet is included (looks good with the Obsidian theme)!

NOTE: bits rA rA and rB rB are the bits representing the register value used

Store
	st  *rA, rB	[0 0 0 0] [rA rA rB rB]

	Store the value of register rB into the memory address represented by rA

Load
	ld  rA, *rB	[0 0 0 1] [rA rA rB rB]

	Load the value at the memory address represented by rB into rA

Add
	add rA, rB	[0 0 1 0] [rA rA rB rB]

	Performs arithmetic addition between values rA and rB, stores result into rA
	Carry bit is lost

Subtract
	sub rA, rB	[0 0 1 1] [rA rA rB rB]

	Performs arithmetic subtraction between values rA and rB, stores result into rA
	Borrow bit is lost

Logical AND
	and rA, rB	[0 1 0 0] [rA rA rB rB]

	Performs bitwise logical AND between values rA and rB, stores result into rA

Logical OR
	or rA, rB	[0 1 0 1] [rA rA rB rB]

	Performs bitwise logical OR between values rA and rB, stores result into rA

Logical NOT
	not rA, rB	[0 1 1 0] [rA rA rB rB]
	[alt] not rA	[0 1 1 0] [rA rA 1 1]

	Performs bitwise logical NOT on values rA and rB, stores results into rA and rB respectively
	Alternate form performs bitwise logical NOT on a single register
	NOTE: will not perform operation on 'pc' register

Clear
	clr rA, rB	[0 1 1 1] [rA rA rB rB]
	[alt] CLR rA	[0 1 1 1] [rA rA 1 1]

	Sets registers rA and rB to 0
	Alternate form sets a single to 0
	NOTE: will not perform operation on 'pc' register

If Equal
	ife rA, rB	[1 0 0 0] [rA rA rB rB]

	Compares values of rA and rB:
		If rA == rB	pc += 2
		Else		pc += 1

If Not Equal
	ifn rA, rB	[1 0 0 1] [rA rA rB rB]

	Compares values of rA and rB:
		If rA != rB	pc += 2
		Else		pc += 1

If Less Than
	ifl rA, rB	[1 0 1 0] [rA rA rB rB]

	Compares values of rA and rB:
		If rA < rB	pc += 2
		Else		pc += 1

If Greater Than
	ifg rA, rB	[1 0 1 1] [rA rA rB rB]

	Compares values of rA and rB:
		If rA > rB	pc += 2
		Else		pc += 1

Increment
	inc rA, value	[1 1 0 0] [rA rA] [v0 v1]

	Adds value (1 to 4) to rA
	NOTE: when encoding, bits v0 v1 are value + 1.
		If value is 3, the encoding is v0 = 1 and v1 = 0

Decrement
	dec rA, value	[1 1 0 1] [rA rA] [v0 v1]

	Subtracts value (1 to 4) from rA
	NOTE: when encoding, bits v0 v1 are value + 1.
		If value is 3, the encoding is v0 = 1 and v1 = 0

Shift Left
	shl rA, value	[1 1 1 0] [rA rA] [v0 v1]

	Shifts value of rA to the left by value (1 to 4)
	NOTE: when encoding bits v0 v1 are value + 1.
		If value is 3, the encoding is v0 = 1 and v1 = 0

Shift Right
	shr rA, value	[1 1 1 1] [rA rA] [v0 v1]

	Shifts value of rA to the right by value (1 to 4)
	NOTE: when encoding bits v0 v1 are value + 1.
		If value is 3, the encoding is v0 = 1 and v1 = 0