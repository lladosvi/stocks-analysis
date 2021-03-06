# stocks-analysis

**Overview of the Project**

The purpose of this project was to refactor a Microsoft Excel VBA code generated as part of the class traing. The code objective was to use certain available stock information in the year 2017 and 2018 and determine whether or not the stocks are worth investing. This process was originally completed in a similar format, however, the goal for this round was to increase the efficiency of the original code.

**Results**

*VBA Code*

I downloaded the provided VBS file, opened it and copied the base provided code to the existing micrsoft excel file. Then I followed the instruction to create additional code and complemented with necessary code I copied and modified from the prior code developed before the refactoring. Final code below:

Sub AllStocksAnalysisRefactored()

    Dim startTime As Single
    Dim endTime  As Single

    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer
    
    'Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate
    
    Range("A1").Value = "All Stocks (" + yearValue + ")"
    
    'Create a header row
    Cells(3, 1).Value = "Ticker"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"

    'Initialize array of all tickers
    Dim tickers(12) As String
    
    tickers(0) = "AY"
    tickers(1) = "CSIQ"
    tickers(2) = "DQ"
    tickers(3) = "ENPH"
    tickers(4) = "FSLR"
    tickers(5) = "HASI"
    tickers(6) = "JKS"
    tickers(7) = "RUN"
    tickers(8) = "SEDG"
    tickers(9) = "SPWR"
    tickers(10) = "TERP"
    tickers(11) = "VSLR"
    
    'Activate data worksheet
    Worksheets(yearValue).Activate
    
    'Get the number of rows to loop over
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row
    
    '1a) Create a ticker Index
    tickerIndex = 0
    
    '1b) Create three output arrays
    Dim tickerVolumes(12) As Long
    Dim tickerStartingPrices(12) As Single
    Dim tickerEndingPrices(12) As Single
    
    ''2a) Create a for loop to initialize the tickerVolumes to zero.
    For i = 0 To 11
    tickerVolumes(i) = 0
    tickerStartingPrices(i) = 0
    tickerEndingPrices(i) = 0
    Next i
        
    ''2b) Loop over all the rows in the spreadsheet.
    For i = 2 To RowCount
    
        '3a) Increase volume for current ticker
         tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value
        
        '3b) Check if the current row is the first row with the selected tickerIndex.
        'If  Then
        If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i - 1, 1).Value <> tickers(tickerIndex) Then
            tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
                       
        'End If
        End If
        
        '3c) check if the current row is the last row with the selected ticker
         'If the next row???s ticker doesn???t match, increase the tickerIndex.
        'If  Then
        If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
            tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
        
        'End If
        End If

            '3d Increase the tickerIndex.
            If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
            tickerIndex = tickerIndex + 1
            End If
             
    Next i
    
    '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    For i = 0 To 11
    
    Worksheets("All Stocks Analysis").Activate
    Cells(4 + i, 1).Value = tickers(i)
    Cells(4 + i, 2).Value = tickerVolumes(i)
    Cells(4 + i, 3).Value = tickerEndingPrices(i) / tickerStartingPrices(i) - 1
    
    Next i
    
    'Formatting
    Worksheets("All Stocks Analysis").Activate
    Range("A3:C3").Font.FontStyle = "Bold"
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    Range("B4:B15").NumberFormat = "#,##0"
    Range("C4:C15").NumberFormat = "0.0%"
    Columns("B").AutoFit

    dataRowStart = 4
    dataRowEnd = 15

    For i = dataRowStart To dataRowEnd
        
        If Cells(i, 3) > 0 Then
            
            Cells(i, 3).Interior.Color = vbGreen
            
        Else
        
            Cells(i, 3).Interior.Color = vbRed
            
        End If
        
    Next i
 
    endTime = Timer
    MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

End Sub

*Stock Performance Analysis and Excecution Times*

Below is the performance of the stocks in 2017 and the respective macro execution time:
![VBA_Challenge_2017](https://user-images.githubusercontent.com/96096924/147141396-c2f5728f-a436-4cd3-9889-e658c00087ab.png)

Below is the performance of the stocsk in 2018 and the respective macro execution time:
![VBA_Challenge_2018](https://user-images.githubusercontent.com/96096924/147141479-c273df97-5fe3-493c-8b92-4a778e084713.png)

**Summary**

Refactoring the code helped reduce sigificantly the processing times, In the 2017 analysis the macro run time improved from 1.23 seconds to 0.18 seconds, while in the 2018 analysis the macro run time improved from 1.16 seconds to 0.17 seconds. So in average times improved between 6 and 7 times. On a performance basis the outcome in both cases is the same so I only see an advantage to the refactoring, and no disadvantage once the code is developed and tested.

From a VBA code perspective the refactoring made some of the parts a bit simpler while some other a bit more complex. On the advantage side and what helped reduce the time significanly was the fact that the loops were independant vs a loop inside of another loop. On the other side the language in general and the indexing was a lot more complex, which led to a much longer programing time and a lot more debugging iterations until the code was ready and performing. So for me the code building process took a lot longer in the refactoring but once built the processing times were much shorter. Given we are going from 1 second to a fraction of a second I would not recommend spending the extra time refactoring in thie particular case with 12 stocks only but in a much larger dataset I do see the value of going through this excersise. 
