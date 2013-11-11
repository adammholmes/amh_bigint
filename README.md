amh_bigint
----------

A portable arbitrary-precision integer arithmetic library written in C as a
personal exercise. Integers are stored as arrays of chars, each representing
a digit. Various arithmetic functions are implemented for use on these 
integers.

Some implementation specifics:
* Multiplication is scaled based on the size of the inputs. For integers that are greater than 10,000 digits, the function will multiply using fast Fourier transforms (specifically a Number-theoretic transform performed using a modified Cooley-Tukey FFT algorithm). For numbers between 10,000 and 750 digits, the Karatsuba algorithm is used. For anything smaller, the function reverts to long multiplication.
* Addition and subtraction are implemented using the "grade-school methods".
* Divison is also performed using the "grade-school method", with a few improvements.
* The greatest common divisor function uses a subtraction-based version of the Euclidean algorithm.
* The power function is performed using exponentiation by squaring.

example
-------

####Code:
```
// Create a few bigints
amhbi_t *one              = amhbi_init_int(1);
amhbi_t *negativetwo      = amhbi_init_str("-2");
amhbi_t *fourquadrillion  = amhbi_init_str("4000000000000000");
amhbi_t *morethan64bits   = amhbi_init_str("18446744073709551617");
amhbi_t *nine             = amhbi_init_str("9");

// Do some simple math
amhbi_t *result1 = amhbi_add_seq(3, one, negativetwo, morethan64bits);
amhbi_t *result2 = amhbi_mult(negativetwo, morethan64bits);
amhbi_t *result3 = amhbi_quo(morethan64bits, fourquadrillion);
amhbi_t *result4 = amhbi_rem(morethan64bits, fourquadrillion);
amhbi_t *result5 = amhbi_pow(morethan64bits, nine);
amhbi_free(5, one, negativetwo, fourquadrillion, morethan64bits, nine);

// Convert and print results
printf("result1 = %s\n", amhbi_to_str(result1));
printf("result2 = %s\n", amhbi_to_str(result2));
printf("result3 = %s\n", amhbi_to_str(result3));
printf("result4 = %s\n", amhbi_to_str(result4));
printf("result5 = %s\n", amhbi_to_str(result5));

// Free our bigint results
amhbi_free(5, result1, result2, result3, result4, result5);

```

####Output:
```
result1 = 18446744073709551616
result2 = -36893488147419103234
result3 = 4611
result4 = 2744073709551617
result5 = 247330401473104534181172792389130563957463768159706303124462774
0826940649968755828834938257320616755882579174087720900647349756190733934
36327809237926297427121301486248656897
```

functions
---------

####Initialization functions
<table>
  <tr>
    <th>Function Name</th><th>Description</th>
  </tr>
  <tr>
    <td>amhbi_init_zero</td><td>Returns a zero valued bigint</td>
  </tr>
  <tr>
    <td>amhbi_init_cpy</td><td>Returns an exact copy of the given bigint</td>
  </tr>
  <tr>
    <td>amhbi_init_str</td><td>Returns the bigint representation of the given string</td>
  </tr>
  <tr>
    <td>amhbi_init_int</td><td>Returns the bigint representation of the given signed int</td>
  </tr>
  <tr>
    <td>amhbi_init_uint</td><td>Returns the bigint representation of the given unsigned int</td>
  </tr>
</table>
####Arithmetic functions
<table>
  <tr>
    <th>Function Name</th><th>Description</th>
  </tr>
  <tr>
    <td>amhbi_add</td><td>Sum two given bigints</td>
  </tr>
  <tr>
    <td>amhbi_add_seq</td><td>Sum a sequence of bigints</td>
  </tr>
  <tr>
    <td>amhbi_subt</td><td>Subtract two given bigints</td>
  </tr>
  <tr>
    <td>amhbi_subt_seq</td><td>Subtract a sequence of bigints from one another</td>
  </tr>
  <tr>
    <td>amhbi_mult</td><td>Multiply two given bigints</td>
  </tr>
  <tr>
    <td>amhbi_mult_seq</td><td>Multiply a sequence of bigints together</td>
  </tr>
  <tr>
    <td>amhbi_mult_pow10</td><td>Multiply a bigint by the given power of 10</td>
  </tr>
  <tr>
    <td>amhbi_pow</td><td>Raise the given bigint by the given power</td>
  </tr>
  <tr>
    <td>amhbi_quo</td><td>Divide two bigints; return the quotient</td>
  </tr>
  <tr>
    <td>amhbi_rem</td><td>Divide two bigints; return the remainder</td>
  </tr>
  <tr>
    <td>amhbi_half</td><td>Quickly divide the given bigint by two; return the quotient</td>
  </tr>
  <tr>
    <td>amhbi_gcd</td><td>Returns the greatest common divisor of the two given bigints</td>
</table>
####Utility functions
<table>
  <tr>
    <th>Function Name</th><th>Description</th>
  </tr>
  <tr>
    <td>amhbi_free</td><td>Destroys each of the given bigints</td>
  </tr>
  <tr>
    <td>amhbi_size</td><td>Returns the size in digits of the given bigint</td>
  </tr>
  <tr>
    <td>amhbi_size_max</td><td>Given a sequence of bigints, returns the size of the largest</td>
  </tr>
  <tr>
    <td>amhbi_size_min</td><td>Given a sequence of bigints, returns the size of the smallest</td>
  </tr>
  <tr>
    <td>amhbi_sign</td><td>Returns the sign of of the given bigint; 0 is positive, 1 is negative</td>
  </tr>
  <tr>
    <td>amhbi_ispos</td><td>Checks if the given bigint is positive</td>
  </tr>
  <tr>
    <td>amhbi_isneg</td><td>Checks if the given bigint is negative</td>
  </tr>
  <tr>
    <td>amhbi_iseven</td><td>Checks if the given bigint is even</td>
  </tr>
  <tr>
    <td>amhbi_isodd</td><td>Checks if the given bigint is odd</td>
  </tr>
  <tr>
    <td>amhbi_iszero</td><td>Checks if the given bigint is zero</td>
  </tr>
  <tr>
    <td>amhbi_isunit</td><td>Checks if the given bigint is either 1 or -1</td>
  </tr>
  <tr>
    <td>amhbi_abs</td><td>Returns a copy of the absolute value of the given bigint</td>
  </tr>
  <tr>
    <td>amhbi_negate</td><td>Returns a copy of the additive inverse of the given bigint</td>
  </tr>
  <tr>
    <td>amhbi_decr</td><td>Decrements the given bigint by one</td>
  </tr>
  <tr>
    <td>amhbi_incr</td><td>Increments the given bigint by one</td>
  </tr>
  <tr>
    <td>amhbi_max</td><td>Given a sequence of bigints, returns a copy of the largest</td>
  </tr>
  <tr>
    <td>amhbi_min</td><td>Given a sequence of bigints, returns a copy of the smallest</td>
  </tr>
  <tr>
    <td>amhbi_cmp</td><td>Compare two given bigints</td>
  </tr>
</table>
</table>
####Conversion functions
<table>
<table>
  <tr>
    <th>Function Name</th><th>Description</th>
  </tr>
  <tr>
    <td>amhbi_to_str</td><td>Returns the string representation of the given bigint</td>
  </tr>
  <tr>
    <td>amhbi_to_int</td><td>Returns the signed int representation of the given bigint; truncates!</td>
  </tr>
  <tr>
    <td>amhbi_to_uint</td><td>Returns the unsigned int representation of the given bigint; truncates!</td>
  </tr>
</table>
