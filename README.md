
The code makes use of the trick that (1<<n)-1 creates a mask of n ones. The only challenge is to avoid
shifting by w when n = w. Instead of writing 1<<n, we write 2<<(n-1). This code will not work for
n = 0, but that’s not a very useful case, anyhow.
Problem 2.69 Solution:
code/data/bits.c
1 /*
2 * Do rotating left shift. Assume 0 <= n < w
3 * Examples when x == 0x12345678:
4 * n == 4 -> 0x23456781, n == 20 -> 0x67812345
5 */
6 unsigned rotate_left(unsigned x, int n) {
7 /* Mask all 1’s when n == 0 and all 0’s otherwise */
8 int z_mask = -!n;
9 /* Left w-n bits */
10 unsigned left = x << n;
11 /* Right n bits */
12 unsigned right = x >> ((sizeof(unsigned)<<3)-n);
13 return (z_mask&x) | (˜z_mask &(left|right));
14 }
code/data/bits.c
For the most part, this problem requires simple shifting and masking. We must treat the case of n = 0 as
special, because we would otherwise attempt to shift by w. Instead, we generate this solution explicitly, and
use masks of all ones and all zeros to select between the special and general case.
Problem 2.70 Solution:
The code is as follows:
code/data/bits.c
1 /*
2 * Return 1 when x can be represented as an n-bit, 2’s complement number;
3 * 0 otherwise
4 * Assume 1 <= n <= w
5 */
6 int fits_bits(int x, int n) {
7 /*
8 * Use left shift then right shift
9 * to sign extend from n bits to full int
10 */
