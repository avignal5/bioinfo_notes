[Home](../index.md) 

# Unix Less Commands

1. No line wrap
   * -S
2. Search forward
   * / – search for a pattern which will take you to the next occurrence.
   * n – for next match in forward
   * N – for previous match in backward
   * &pattern – display only the matching lines, not all.
3. Search backwards
   * ? – search for a pattern which will take you to the previous occurrence.
   * n – for next match in backward direction
   * N – for previous match in forward direction
4. Screen Navigation
   * CTRL+F – forward one window
   * CTRL+B – backward one window
   * CTRL+D – forward half window
   * CTRL+U – backward half window
5. Line navigation
   * j – navigate forward by one line
   * k – navigate backward by one line
   * 10j – 10 lines forward.
   * 10k – 10 lines backward.
6. Other navigation
   * G – go to the end of file
   * g – go to the start of file
   * gN : go to line N
   * CTRL+G – show the current file name along with line, byte and percentage statistics.
   * q or ZZ – exit the less pager
7. Multiple file paging
   * less file1 file2
   * less file1 ; then :e file2
   * :n – go to the next file.
   * :p – go to the previous file.
8. Marked navigation
   * ma – mark the current position with the letter ‘a’,
   * ‘a – go to the marked position ‘a’.
9. Help
   * h : help
   * v – using the configured editor edit the current file




* See also [Unix Less Command: 10 Tips for Effective Navigation](https://www.thegeekstuff.com/2010/02/unix-less-command-10-tips-for-effective-navigation/)


