; Author : Matthew Romleski
; Tech ID: 12676184
; Program which runs through a size 40 array of 8-bit elements and counts the number that have either the 7, 6, and 0 bit set to 1.

		.include <atxmega128A1Udef.inc>

		.dseg
		.def	countReg = r0
		.def	storeReg = r16
		.def	loopConReg = r17

		.cseg
		.org	0x00
		rjmp	start
		.org	0xF6

start:	ldi		ZL, low(array << 1)
		ldi		ZH, high(array << 1)
		ldi		loopConReg, 1

loop:	cpi		loopConReg, 41 ; Sees if the loop has gone through all 40 of the elements in the array.
		breq	done ; Branches if it has.
		lpm		storeReg, Z+ ; Loads the number to be tested from Program Memory.
		andi	storeReg, 193 ; 193 in binary is 1100 0001, so we AND this with our number.
		cpi		storeReg, 193 ; If the result of the AND is 193, then the 7 && 6 && 0 bit was 1.
		brne	loopCn ; If the storeReg =/= 193, the CPI sets Z in SREG to 0, skipping this branch.
		inc		countReg ; Meaning this only increments if storeReg = 193.

loopCn:	inc		loopConReg
		jmp		loop

done:	rjmp	done

array:	.db		 11,  12,  13,  14,  15,  16,  17,  18,  19,  20 ; Only the final row is >193,
		.db		 21,  22,  23,  24,  25,  26,  27,  28,  29,  30 ; and only 1/2 of those are odd (0th bit = 1).
		.db		 31,  32,  33,  34,  35,  36,  37,  38,  39,  40 ; So using this array of numbers,
		.db		201, 202, 203, 204, 205, 206, 207, 208, 209, 210 ; We should see a result of 5 in our countReg.