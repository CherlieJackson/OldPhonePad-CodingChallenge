

# OldPhonePad Coding Challenge

## 📌 Problem Description
This project is a solution to the **Old Phone Pad Coding Challenge** from Iron Software.  
The challenge is to simulate typing text using a traditional mobile phone keypad.

Each button has multiple letters, and pressing the button multiple times cycles through them.  
For example:
- `2` → A  
- `22` → B  
- `222` → C  

Rules:
- `*` = Backspace (delete last character)  
- `#` = Send (end input, return final result)  
- A space `" "` means a pause (used to type two characters from the same key in sequence).  

---

## 📌 Examples
| Input | Output |
|-------|---------|
| `33#` | E |
| `227*#` | B |
| `4433555 555666#` | HELLO |
| `8 88777444666*664#` | ???? |

---

## 📌 How the Code Works
1. Define a mapping of keypad numbers to letters (e.g. `2 -> "ABC"`, `3 -> "DEF"`).  
2. Loop through each character of the input string.  
3. Handle the following:
   - If same key pressed → count how many times.  
   - If space → confirm previous letter.  
   - If `*` → remove last character.  
   - If `#` → stop and return result.  
4. Append the correct letter(s) to the result.  

---

## 📌 Code Example

```csharp
using System;
using System.Text;
using System.Collections.Generic;

public class OldPhone
{
    public static string OldPhonePad(string input)
    {
        Dictionary<char, string> keypad = new Dictionary<char, string>
        {
            {'2', "ABC"},
            {'3', "DEF"},
            {'4', "GHI"},
            {'5', "JKL"},
            {'6', "MNO"},
            {'7', "PQRS"},
            {'8', "TUV"},
            {'9', "WXYZ"}
        };

        StringBuilder result = new StringBuilder();
        char lastKey = '\0';
        int count = 0;

        foreach (char c in input)
        {
            if (c == '#')
            {
                if (lastKey != '\0')
                    result.Append(keypad[lastKey][(count - 1) % keypad[lastKey].Length]);
                break;
            }
            else if (c == '*')
            {
                if (result.Length > 0) result.Remove(result.Length - 1, 1);
                lastKey = '\0';
                count = 0;
            }
            else if (c == ' ')
            {
                if (lastKey != '\0')
                {
                    result.Append(keypad[lastKey][(count - 1) % keypad[lastKey].Length]);
                    lastKey = '\0';
                    count = 0;
                }
            }
            else if (keypad.ContainsKey(c))
            {
                if (c == lastKey)
                {
                    count++;
                }
                else
                {
                    if (lastKey != '\0')
                        result.Append(keypad[lastKey][(count - 1) % keypad[lastKey].Length]);

                    lastKey = c;
                    count = 1;
                }
            }
        }

        return result.ToString();
    }
}
