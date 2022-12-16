# WarmUp №5. Long arithmetic (residual)
---
### Task:
![The task](https://i.imgur.com/NuAldiZ.png)

>The program calculates the residual of the numbers.

#### Language: Delphi

### Code:
``` pascal
Program WarmUp5;
{
 The program should calculate the residual of n numbers in the entered number system
}

{$APPTYPE CONSOLE}

Uses
  System.SysUtils;

Const
  MaxSize=256;
  MaxAmount=50;
  NSAlphabet = '0123456789ABCDEFGHIJ';
  MaxNS = length(NSAlphabet);
  //MaxSize - maximum amount of digits for entered numbers
  //MaxAmount - maximum amount of numbers
  //NSAlphabet - transfer between symbols and numbers
  //MaxNS - maximum number system

Var
  Num1:array[1..(MaxSize + (MaxAmount div 10) + 1)] of ShortInt;
  Num2 :array[1..MaxSize] of ShortInt;
  str :string;
  NS, Amount : Integer;
  j, Res, Carry, Sign1, Sign2 : ShortInt;
  Len1, Len2, Size, i : Word;
  Minus, ZerosRemoved, FoundLarger, flag: boolean;
  //Num1 - array of digits of the first number (further the residual of the two previous numbers)
  //Num2 - array of digits of the second numbers (with which need to subtract the first number)
  //str - string variable for writing numbers
  //NS - number system
  //Amount - amount of numbers
  //Res - residual of digits
  //Carry - сarry 1 (if there is) to the next element
  //Sign1, Sign2 - the variable stores the sign in order to subtract the fewer number from the larger number
  //Len1 - the length of the first entered number
  //Len2 - the length of the second entered number
  //Size - the size of the residual
  //i, j - cycle counter
  //Minus - if b is greater than a, then the answer will be with a minus
  //ZerosRemoved - variable to remove the unnecessary zeros (if there is)
  //FoundLarger - when a larger number is found, the variable will become true
  //flag - flag to confirm the correctness of entering numbers

Begin

  Writeln('Enter number system (maximum number system is ',MaxNS,', minimal is 2). The program will calculate their residual.');

  //Cycle with postcondition for entering correct data.
  Repeat

    //Initialize the flag
    flag:= False;

    //Validating the correct input data type
    Try
      Readln(NS);
    Except
      Writeln('Wrong input of number system! It must be an integer');
      flag:= True;
    End;

    //Validate Range
    if ((NS > MaxNS) or (NS < 2)) and (flag = False) then
    begin
      Writeln('Wrong input of number system! It must be >=2 and <=',MaxNS);
      flag:= True;
    end;

  Until flag = False;

  Writeln;
  Writeln('Enter amount of numbers (no more than ',MaxAmount,' and more than 1)');

  //Cycle with postcondition for entering correct data.
  Repeat

    //Initialize the flag
    flag:= False;

    //Validating the correct input data type
    Try
      Readln(Amount);
    Except
      Writeln('Wrong input of amount of numbers! It must be an integer');
      flag:= True;
    End;

    //Validate Range
    if ((Amount > MaxAmount) or (Amount <= 1)) and (flag = False) then
    begin
      Writeln('Wrong input of amount of numbers! It must be >1 and <=',MaxAmount);
      flag:= True;
    end;

  Until flag = False;

  //Declaring available symbols and their value
  Writeln;
  Writeln('Available symbols on the ',NS,'th number system and their number system:');
  for i := 0 to (NS - 1) do
    Writeln('Symbol ', NSAlphabet[i+1],' Value = ',i);
  Writeln;

  Writeln('Enter numbers (no more than ',MaxSize,' digits).');

  //Cycle with postcondition for entering correct data.
  Repeat

    //Initialize the flag
    flag:= False;

    //Read the first entered number and check for correctness.
    Readln(str);

    //Find length of the first number
    Len1:= length(str);

    //Checking the correct length
    if Len1 > MaxSize then
    begin
      Writeln('Wrong input of number! The length of the number must be no more than ',MaxSize,' digits.');
      flag:= True;
    end

    //Else if length >1, the first digit cannot be 0 (in the mirrored view it is last)
    else if (Len1 > 1) and (str[1] = '0') then
    begin
      Writeln('Wrong input of number! The first digit of a number cannot be 0');
      flag:= True;
    end

    //Else writing a number to an array and checking for valid symbols
    else
    begin

      //Reset the first number for the input
      for i := 1 to length(Num1) do
        Num1[i]:= 0;

      //Write the first entered number in mirrored view to an array
      i:=1;
      while (i <= Len1) and (flag = False) do
      begin

        //Transfer to numerical value (-1 because numbering in delphi starts from 1)
        Num1[i]:= Pos(str[Len1-i+1], NSAlphabet) - 1;

        //Checking for correct input in the number system
        //Num1[i] will be <0 if the symbol is not in NSAlphabet
        if (Num1[i] < 0) or (Num1[i] >= NS) then
        begin
          Writeln('Wrong input of number! Namely, wrong input of symbols! See available symbols above!');
          flag:= True;
        end;

        //Modernize i
        i:= i + 1;
      end;

    end;

  Until flag = False;



  //The cycle go (Amount-1) times to subtract all the numbers
  j:=1;
  for j := 1 to (Amount - 1) do
  begin

    Writeln('-');

    //Cycle with postcondition for entering correct data.
    Repeat

      //Initialize the flag
      flag:= False;

      //Read the second entered number and check for correctness.
      Readln(str);

      //Find length of the second number
      Len2:= length(str);

      //Checking the correct length
      if Len2 > MaxSize then
      begin
        Writeln('Wrong input of number! The length of the number must be no more than ',MaxSize,' digits.');
        flag:= True;
      end

      //Else if length >1, the first digit cannot be 0 (in the mirrored view it is last)
      else if (Len2 > 1) and (str[1] = '0') then
      begin
        Writeln('Wrong input of number! The first digit of a number cannot be 0');
        flag:= True;
      end

      //Else writing a number to an array and checking for valid symbols
      else
      begin

        //Reset the second number for the input
        for i := 1 to length(Num2) do
          Num2[i]:= 0;

        //Write the second number in mirrored view to an array
        i:=1;
        while (i <= Len2) and (flag = False) do
        begin

          //Transfer to numerical value (-1 because numbering in delphi starts from 1)
          Num2[i]:= Pos(str[Len2-i+1], NSAlphabet) - 1;

          //Checking for correct input in the number system
          //Num2[i] will be <0 if the symbol is not in NSAlphabet
          if (Num2[i] < 0) or (Num2[i] >= NS) then
          begin
            Writeln('Wrong input of number! Namely, wrong input of symbols! See available symbols above!');
            flag:= True;
          end;

          //Modernize i
          i:= i + 1;
        end;

      end;

    Until flag = False;

    //Compare the sizes of the first and second numbers.
    //The larger size assign in a Size (this is the size of the residual of the numbers)
    if Len1 > Len2 then
      Size:= Len1
    else
      Size:= Len2;

    //Searching the larger number. To do this, compare the first digits (in the mirrored view it is last).
    //If they are equal, move on.
    //If the residual is negative, then there is no need to compare further (since will be the sum of negative numbers)
    i:= Size;
    FoundLarger:= False;
    while (i >= 1) and (FoundLarger = False) and (Minus = False) do
    begin

      //If Num1>Num2, the sign for the first number is '+', for the second '-' (Num1-Num2).
      if Num1[i] > Num2[i] then
      begin

        Sign1:= 1;
        Sign2:= -1;

        //Exit the cycle because found a larger number
        FoundLarger:= True;
      end

      //If Num2>Num1, the sign for the first number is '-', for the second '+' (Num2-Num1).
      else if Num2[i] > Num1[i] then
      begin

        Sign2:= 1;
        Sign1:= -1;

        //Variable Minus since the answer is negative (Num1<Num2)
        Minus:= True;

        //Exit the cycle because found a larger number.
        FoundLarger:= True;
      end;

      //Modernize i
      i:= i - 1;
    end;

    //This condition is needed if the user entered two identical numbers. Their subtraction will be 0.
    if (FoundLarger = False) and (Minus = False) then
    begin

      //Since the numbers are the same, the answer will be 0
      for i := 1 to Len1 do
        Num1[i]:= 0;

      //The size of the subtract will be 1 (0 is one digit).
      Size:=1;

      //Note that compared numbers and supposedly found the largest
      FoundLarger:= True;

    end;

    //In the cycle will subtract the numbers separately
    for i := 1 to Size do
    begin

      //If found the largest number, then need to do the subtract of numbers
      if (FoundLarger = True) then
      begin

        //Considering the signs, starting to subtract the last digits of the numbers (in the mirrored view it is first)
        //and consider the carry (if there is).
        Res:= Sign1*Num1[i] + Sign2*Num2[i] + Carry;

        //If the difference is less than zero, then take 1 (for the current digit it is NS) from the next digit
        if Res < 0 then
        begin
          Res:= Res + NS;
          Carry:= -1;
        end
        //Else carry = 0
        else
          Carry:= 0;

        //Put the result in the residual array c
        Num1[i]:= Res;

      end

      //If did not compare the numbers, then the first and second numbers are negative, so need to add them
      else
      begin

        //Starting to add (add negative numbers) the last digits of the numbers (in the mirrored view it is first)
        //and add the carry (if there is).
        Res:= Carry + Num1[i] + Num2[i];

        //The modulo of the Sum by number system is the last(in the mirrored it is first) digit of sums (array Num1).
        Num1[i]:= Res mod NS;

        //The integer part of dividing by number system is the carry that will go to the next element
        Carry:= Res div NS;

        //If there is a carry on the last digit, then increase the size of sum by 1 and add a carry to the next element
        if (i = Size) and (Carry = 1) then
        begin
          Size:= Size + 1;
          Num1[i+1]:= Carry;
        end;

      end;

    end;

    //Update the length of the first number
    Len1:= Size;

    //Reset the carry for the following operations
    Carry:= 0;

  end;





  Writeln('The residual of the numbers is:');

  //If the second number is greater than the first, then write the sign -
  if Minus = True then
    Write('-');

  //If size = 1, no need to remove zeros
  if size = 1 then
    ZerosRemoved:= True;

  //Write the answer, mirroring the back. Аlso remove unnecessary zeros(if there is)
  //And transfer to symbolic value (+1 because numbering in delphi starts from 1)
  for i := Size downto 1 do
  begin
    if (ZerosRemoved = False) and (Num1[i] > 0) then
      ZerosRemoved:= True;
    if (ZerosRemoved = True) then
      Write(NSAlphabet[Num1[i]+1]);
  end;

  Readln;
End.
```