# barcode-generator-powerbi-report-builder-code128
This Code will allow to create a Code 128 barcode in a Power Bi Report Builder. This will also work in Power Bi Online without using any external resources

## How it Works
  1. Input Processing: The input is trimmed to 18 digits for this example but it can be expanded more.
  2. Generation of Code 128: Each digit is converted to its corresponding pattern, all required patters are already populated in the script.
  3. Barcode Visualization: The barcode is represented by a series of 254 rectangles, colored black or white based on the generated pattern.

## Implementation
  1. Add a column to place the barcode
  2. Copy the code to the Report Code. To access the report code, click over the grey area in Report Builder and select Report Properties, then in the report properties select Code and paste the code in the "Custom code" field.
  3. Create a rectangle for each bar (254 rectangles in this example) and assign the following expression for the background color( Rectangle Properties>Fill>Fill color>Fx: =Code.GetBarcodeColor(Fields!your_var.Value.ToString(), N) Where your_var = field to be converted to Barcode N = rectangle position, Nums from 1 to 254
  4. This code will work for up to 18 characters if aditional characters are required calculate the required amount of rectangles and adjust the Trim part of the code. To calculate the ammount of required rectangles, multiple the ammount of caracter by 11 then add 55 ie (18 x 11) + 55 = 254.

### Code

Add the following VB.NET code to your report:
```vb.net
Function ProcessInput(ByVal data As String) As String
                  data = Trim(data)
                  data = System.Text.RegularExpressions.Regex.Replace(data, "[^\d]", "")
                  If Len(data) > 18 Then
                      data = Right(data, 18)  ' Take the last 20 digits
                  End If
                  Return data
              End Function
              
              Function GenerateCode128BPattern(ByVal data As String) As String
                  Dim codePatterns(106) As String
                  ' Tabla completa de patrones Code 128 (Subset B)
                  ' Cada uno representa 11 módulos (barras y espacios)
                  codePatterns(0) = "11011001100"
                  codePatterns(1) = "11001101100"
                  codePatterns(2) = "11001100110"
                  codePatterns(3) = "10010011000"
                  codePatterns(4) = "10010001100"
                  codePatterns(5) = "10001001100"
                  codePatterns(6) = "10011001000"
                  codePatterns(7) = "10011000100"
                  codePatterns(8) = "10001100100"
                  codePatterns(9) = "11001001000"
                  codePatterns(10) = "11001000100"
                  codePatterns(11) = "11000100100"
                  codePatterns(12) = "10110011100"
                  codePatterns(13) = "10011011100"
                  codePatterns(14) = "10011001110"
                  codePatterns(15) = "10111001100"
                  codePatterns(16) = "10011101100"
                  codePatterns(17) = "10011100110"
                  codePatterns(18) = "11001110010"
                  codePatterns(19) = "11001011100"
                  codePatterns(20) = "11001001110"
                  codePatterns(21) = "11011100100"
                  codePatterns(22) = "11001110100"
                  codePatterns(23) = "11101101110"
                  codePatterns(24) = "11101001100"
                  codePatterns(25) = "11100101100"
                  codePatterns(26) = "11100100110"
                  codePatterns(27) = "11101100100"
                  codePatterns(28) = "11100110100"
                  codePatterns(29) = "11100110010"
                  codePatterns(30) = "11011011000"
                  codePatterns(31) = "11011000110"
                  codePatterns(32) = "11000110110"
                  codePatterns(33) = "10100011000"
                  codePatterns(34) = "10001011000"
                  codePatterns(35) = "10001000110"
                  codePatterns(36) = "10110001000"
                  codePatterns(37) = "10001101000"
                  codePatterns(38) = "10001100010"
                  codePatterns(39) = "11010001000"
                  codePatterns(40) = "11000101000"
                  codePatterns(41) = "11000100010"
                  codePatterns(42) = "10110111000"
                  codePatterns(43) = "10110001110"
                  codePatterns(44) = "10001101110"
                  codePatterns(45) = "10111011000"
                  codePatterns(46) = "10111000110"
                  codePatterns(47) = "10001110110"
                  codePatterns(48) = "11101110110"
                  codePatterns(49) = "11010001110"
                  codePatterns(50) = "11000101110"
                  codePatterns(51) = "11011101000"
                  codePatterns(52) = "11011100010"
                  codePatterns(53) = "11011101110"
                  codePatterns(54) = "11101011000"
                  codePatterns(55) = "11101000110"
                  codePatterns(56) = "11100010110"
                  codePatterns(57) = "11101101000"
                  codePatterns(58) = "11101100010"
                  codePatterns(59) = "11100011010"
                  codePatterns(60) = "11101111010"
                  codePatterns(61) = "11001000010"
                  codePatterns(62) = "11110001010"
                  codePatterns(63) = "10100110000"
                  codePatterns(64) = "10100001100"
                  codePatterns(65) = "10010110000"
                  codePatterns(66) = "10010000110"
                  codePatterns(67) = "10000101100"
                  codePatterns(68) = "10000100110"
                  codePatterns(69) = "10110010000"
                  codePatterns(70) = "10110000100"
                  codePatterns(71) = "10011010000"
                  codePatterns(72) = "10011000010"
                  codePatterns(73) = "10000110100"
                  codePatterns(74) = "10000110010"
                  codePatterns(75) = "11000010010"
                  codePatterns(76) = "11001010000"
                  codePatterns(77) = "11110111010"
                  codePatterns(78) = "11000010100"
                  codePatterns(79) = "10001111010"
                  codePatterns(80) = "10100111100"
                  codePatterns(81) = "10010111100"
                  codePatterns(82) = "10010011110"
                  codePatterns(83) = "10111100100"
                  codePatterns(84) = "10011110100"
                  codePatterns(85) = "10011110010"
                  codePatterns(86) = "11110100100"
                  codePatterns(87) = "11110010100"
                  codePatterns(88) = "11110010010"
                  codePatterns(89) = "11011011110"
                  codePatterns(90) = "11011110110"
                  codePatterns(91) = "11110110110"
                  codePatterns(92) = "10101111000"
                  codePatterns(93) = "10100011110"
                  codePatterns(94) = "10001011110"
                  codePatterns(95) = "10111101000"
                  codePatterns(96) = "10111100010"
                  codePatterns(97) = "11110101000"
                  codePatterns(98) = "11110100010"
                  codePatterns(99) = "10111011110"
                  codePatterns(100) = "10111101110"
                  codePatterns(101) = "11101011110"
                  codePatterns(102) = "11110101110"
                  codePatterns(103) = "11010000100" ' Start A
                  codePatterns(104) = "11010010000" ' Start B
                  codePatterns(105) = "11010011100" ' Start C
                  codePatterns(106) = "1100011101011" ' Stop (13 modules)
              
                  Dim result As String = ""
                  Dim checksum As Integer = 104 ' Start B
                  result &= codePatterns(104) ' Start B pattern
              
                  For i As Integer = 1 To Len(data)
                      Dim codeValue As Integer = Asc(Mid(data, i, 1)) - 32
                      If codeValue < 0 Or codeValue > 94 Then
                          Return "ERROR"
                      End If
                      checksum += codeValue * i
                      result &= codePatterns(codeValue)
                  Next
              
                  checksum = checksum Mod 103
                  result &= codePatterns(checksum)
                  result &= codePatterns(106) ' Stop
              
                  ' Add quiet zones
                  Return New String("0"c, 10) & result & New String("0"c, 10)
              End Function
              
              Function GetBarcodeColor(ByVal data As String, ByVal index As Integer) As String
                  Dim pattern As String = GenerateCode128BPattern(data)
                  If index >= 1 AndAlso index <= Len(pattern) Then
                      Return IIf(Mid(pattern, index, 1) = "1", "Black", "White")
                  Else
                      Return "White"
                  End If
              End Function
