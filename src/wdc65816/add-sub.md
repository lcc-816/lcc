#pragma mark - add / subtract

reg: ADDU2(rc, rc) {
    lda %0
    clc
    adc %1
    sta %c
} 4+1

reg: ADDI2(rc, rc) {
    lda %0
    clc
    adc %1
    sta %c
} 4+1


reg: ADDU2(rc, const_1) {
    lda %0
    inc
    sta %c
} 3

reg: ADDI2(rc, const_1) {
    lda %0
    inc
    sta %c
} 3

reg: ADDU2(rc, const_2) {
    lda %0
    inc
    inc
    sta %c
} 4

reg: ADDI2(rc, const_2) {
    lda %0
    inc
    inc
    sta %c
} 4

# add, or, and, eor - should prefer
# lda dp
# and #constant
# vs
# lda #constant
# and dp
# since it will make peephole optimizations easier.
# simplify() moves a constant to the right child.

reg: ADDU2(reg, const_1) {
    inc %0
} rmw(a, 1)

reg: ADDI2(reg, const_1) {
    inc %0
} rmw(a, 1)

reg: ADDU2(reg, const_2) {
    inc %0
    inc %0
} rmw(a, 2)

reg: ADDI2(reg, const_2) {
    inc %0
    inc %0
} rmw(a, 2)

reg: SUBU2(reg, const_1) {
    dec %0
} rmw(a, 1)

reg: SUBI2(reg, const_1) {
    dec %0
} rmw(a, 1)

reg: SUBU2(reg, const_2) {
    dec %0
    dec %0
} rmw(a, 2)

reg: SUBI2(reg, const_2) {
    dec %0
    dec %0
} rmw(a, 2)


reg: SUBU2(rc, rc) {
    lda %0
    sec
    sbc %1
    sta %c
} 4

reg: SUBI2(rc, rc) {
    lda %0
    sec
    sbc %1
    sta %c
} 4

reg: SUBU2(rc, const_1) {
    lda %0
    dec
    sta %c
} 3

reg: SUBI2(rc, const_1) {
    lda %0
    dec
    sta %c
} 3
