Source code copied from
https://stackoverflow.com/questions/8207997/calling-a-fortran-subroutine-from-c
# gcc-compiled-code-calling-gfortran-calling-subroutine
(00:48 ian@ian-Aspire-One-753 ~) > gfortran myfortsub.f90 -c
(00:49 ian@ian-Aspire-One-753 ~) > gfortran myfortsub.f -ffree-form -c
(00:50 ian@ian-Aspire-One-753 ~) > gfortran   \
     testfort.o  \
     myfortsub.f90  \
     -o test_fortsube
(00:52 ian@ian-Aspire-One-753 ~) > ./test_fortsube
 >abcd<           4
(00:53 ian@ian-Aspire-One-753 ~) > cat testfort.c
int main ( void ) {

   char test [10] = "abcd";

   myfortsub (test);

   return 0;

}
(00:54 ian@ian-Aspire-One-753 ~) > cat myfortsub.f90
       subroutine myfortsub ( input_string ) bind ( C, name="myfortsub" )

       use iso_c_binding, only: C_CHAR, c_null_char
       implicit none

  character (kind=c_char, len=1), dimension (10), intent (in) :: input_string
  character (len=10) :: regular_string
   integer :: i

   regular_string = " "
   loop_string: do i=1, 10
      if ( input_string (i) == c_null_char ) then
         exit loop_string
      else
         regular_string (i:i) = input_string (i)
      end if
   end do loop_string

   write (*, *) ">", trim (regular_string), "<", len_trim (regular_string)

   return

end subroutine myfortsub
(00:54 ian@ian-Aspire-One-753 ~) > 
