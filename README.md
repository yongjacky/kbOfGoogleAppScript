# Knowledge Base of Google App Script

## To implement Convert Numbers/Currency to Words into your Google Spreadsheet (Malaysia Format)
*Step 1:* From within your new/existing spreadsheet, select the menu item **Tool > Script editor**. 
If you are presented with a welcome screen, click **Blank Project**.

*Step 2:* Delete any code in the script editor and paste in the code below.
```javascript
aTens = [ "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"];
aOnes = [ "Zero", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine",
  "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", 
  "Nineteen" ];
function ConvertToHundreds(num)
{
   var cNum, nNum;
   var cWords = "";
   num %= 1000;
   if (num > 99) {
      /* Hundreds. */
      cNum = String(num);
      nNum = Number(cNum.charAt(0));
      cWords += aOnes[nNum] + " Hundred";
      num %= 100;
      if (num > 0)
         cWords += " and "
   }

   if (num > 19) {
      /* Tens. */
      cNum = String(num);
      nNum = Number(cNum.charAt(0));
      cWords += aTens[nNum - 2];
      num %= 10;
      if (num > 0)
         cWords += "-";
   }
   if (num > 0) {
      /* Ones and teens. */
      nNum = Math.floor(num);
      cWords += aOnes[nNum];
   }
   return cWords;
}
function ConvertToWords(num)
{
   var aUnits = [ "Thousand", "Million", "Billion", "Trillion", "Quadrillion" ];
   var cWords = (num >= 1 && num < 2) ? "Ringgit and " : "Ringgit and ";
   var nLeft = Math.floor(num);
   for (var i = 0; nLeft > 0; i++) { 
       if (nLeft % 1000 > 0) {
          if (i != 0)
             cWords = ConvertToHundreds(nLeft) + " " + aUnits[i - 1] + " " + cWords;
          else
             cWords = ConvertToHundreds(nLeft) + " " + cWords;
       }
       nLeft = Math.floor(nLeft / 1000);
   }
   num = Math.round(num * 100) % 100;
   if (num > 0)
      cWords += ConvertToHundreds(num) + " Cents";
   else
      cWords += "Zero Cents";
   return cWords;
}
```

*Step 3:* Select the menu item **File > Save**. Name your new script and click **OK**.

*Final Step:* Try it out 
> Switch back to your spreadsheet and reload the page.
> Enter the formula =CONVERTTOWORDS(Cell Number) and press Enter.
> Expect result for example: **1,500.50** will be converted as **One Thousand Five Hundred Ringgit and Fifty Cents**
