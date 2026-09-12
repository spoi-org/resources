---
draft: false
title: "Number Spiral"
editorial:
  platform: "CSES"
  name: "Number Spiral"
---

{{< problem "cses-number-spiral" >}}

## Abridged problem statement
Given a grid with numbers filled in the form of an outward spiral, find the number that is present in the cell (i,j).
## Observation
Consider a square from the top left corner to a cell (a,a). This square contains all the numbers from $1$ to $a^2$. For example a square starting at cell (1,1) and ending at cell (3,3) has values from 1 to 9.

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Colored Grid</title>
<style>
  body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
    margin: 0;
  }

  table {
    border-collapse: collapse;
    font-family: Arial, sans-serif;
    font-weight: bold;
    font-size: 24px;
    background-color: #ffffff;
  }

  td {
    border: 4px solid #1a1a1a; 
    width: 70px;
    height: 70px;
    text-align: center;
    vertical-align: middle;
    color: #000000;
  }

  .fill-blue {
    background-color: #1E90FF; /* Dodger Blue applied to 1-9 */
  }
  
  .fill-default {
    background-color: #dcdcdc; 
  }
</style>
</head>
<body>

<table>
  <tr>
    <td class="fill-blue">1</td>
    <td class="fill-blue">2</td>
    <td class="fill-blue">9</td>
    <td class="fill-default">10</td>
    <td class="fill-default">25</td>
  </tr>
  <tr>
    <td class="fill-blue">4</td>
    <td class="fill-blue">3</td>
    <td class="fill-blue">8</td>
    <td class="fill-default">11</td>
    <td class="fill-default">24</td>
  </tr>
  <tr>
    <td class="fill-blue">5</td>
    <td class="fill-blue">6</td>
    <td class="fill-blue">7</td>
    <td class="fill-default">12</td>
    <td class="fill-default">23</td>
  </tr>
  <tr>
    <td class="fill-default">16</td>
    <td class="fill-default">15</td>
    <td class="fill-default">14</td>
    <td class="fill-default">13</td>
    <td class="fill-default">22</td>
  </tr>
  <tr>
    <td class="fill-default">17</td>
    <td class="fill-default">18</td>
    <td class="fill-default">19</td>
    <td class="fill-default">20</td>
    <td class="fill-default">21</td>
  </tr>
</table>

</body>
</html>

## Solution
Now let us divide the grid into squares starting at the top left corner of the grid. 
Now I pick a cell (x,y) outside a square of dimentions $a \times a$. I know the value on cell (x,y) has to be greater than $a^2$. This is because all the numbers from $1$ to $a^2$ are already present in the square. 
So how do we use this to solve this problem?
Let me consider the largest square the cell (x,y) is not part of. What should be the dimentions of this square? If $x \geq y$ this square must have dimentions $(x-1) \times (x-1)$. If $y \geq x$ this square must have dimentions $(y-1) \times (y-1)$. 
After we fill the square, we need to find how the remaining numbers are filled. These numbers are filled using method 1 and method 2 alternatively.

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Colored Grid with Arrow</title>
<style>
  body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
    margin: 0;
  }

  /* Container to hold both the table and the SVG overlay */
  .grid-container {
    position: relative;
    width: 400px;
    height: 400px;
  }

  table {
    width: 100%;
    height: 100%;
    border-collapse: collapse;
    font-family: Arial, sans-serif;
    font-weight: bold;
    font-size: 24px;
    background-color: #ffffff;
  }

  td {
    border: 4px solid #1a1a1a; 
    width: 20%; 
    height: 20%; 
    text-align: center;
    vertical-align: middle;
    color: #000000;
    box-sizing: border-box;
  }

  .fill-blue {
    background-color: #1E90FF; /* Dodger Blue for 1-9 */
  }
  
  .fill-default {
    background-color: #dcdcdc; /* Light gray for default cells */
  }

  /* SVG overlay settings */
  svg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none; /* Allows mouse interactions to pass through to the table if needed */
  }
</style>
</head>
<body>

<div class="grid-container">
  <table>
    <tr>
      <td class="fill-blue">1</td>
      <td class="fill-blue">2</td>
      <td class="fill-blue">9</td>
      <td class="fill-default">10</td>
      <td class="fill-default">25</td>
    </tr>
    <tr>
      <td class="fill-blue">4</td>
      <td class="fill-blue">3</td>
      <td class="fill-blue">8</td>
      <td class="fill-default">11</td>
      <td class="fill-default">24</td>
    </tr>
    <tr>
      <td class="fill-blue">5</td>
      <td class="fill-blue">6</td>
      <td class="fill-blue">7</td>
      <td class="fill-default">12</td>
      <td class="fill-default">23</td>
    </tr>
    <tr>
      <td class="fill-default">16</td>
      <td class="fill-default">15</td>
      <td class="fill-default">14</td>
      <td class="fill-default">13</td>
      <td class="fill-default">22</td>
    </tr>
    <tr>
      <td class="fill-default">17</td>
      <td class="fill-default">18</td>
      <td class="fill-default">19</td>
      <td class="fill-default">20</td>
      <td class="fill-default">21</td>
    </tr>
  </table>
  
  <!-- SVG to draw the continuous arrow path -->
  <svg>
    <path 
      d="M 255 20 L 255 255 L 35 255 M 55 240 L 35 255 L 55 270" 
      stroke="#0b57d0" 
      stroke-width="8" 
      fill="none" 
      stroke-linecap="round" 
      stroke-linejoin="round" 
    />
  </svg>
</div>

</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Colored Grid with Arrow</title>
<style>
  body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
    margin: 0;
  }

  .grid-container {
    position: relative;
    width: 400px;
    height: 400px;
  }

  table {
    width: 100%;
    height: 100%;
    border-collapse: collapse;
    font-family: Arial, sans-serif;
    font-weight: bold;
    font-size: 24px;
    background-color: #ffffff;
  }

  td {
    border: 4px solid #1a1a1a; 
    width: 20%; 
    height: 20%; 
    text-align: center;
    vertical-align: middle;
    color: #000000;
    box-sizing: border-box;
  }

  .fill-blue {
    background-color: #1E90FF; /* Dodger Blue for 1-16 */
  }
  
  .fill-default {
    background-color: #dcdcdc; /* Light gray for default cells */
  }

  /* SVG overlay settings */
  svg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none; 
  }
</style>
</head>
<body>

<div class="grid-container">
  <table>
    <tr>
      <td class="fill-blue">1</td>
      <td class="fill-blue">2</td>
      <td class="fill-blue">9</td>
      <td class="fill-blue">10</td>
      <td class="fill-default">25</td>
    </tr>
    <tr>
      <td class="fill-blue">4</td>
      <td class="fill-blue">3</td>
      <td class="fill-blue">8</td>
      <td class="fill-blue">11</td>
      <td class="fill-default">24</td>
    </tr>
    <tr>
      <td class="fill-blue">5</td>
      <td class="fill-blue">6</td>
      <td class="fill-blue">7</td>
      <td class="fill-blue">12</td>
      <td class="fill-default">23</td>
    </tr>
    <tr>
      <td class="fill-blue">16</td>
      <td class="fill-blue">15</td>
      <td class="fill-blue">14</td>
      <td class="fill-blue">13</td>
      <td class="fill-default">22</td>
    </tr>
    <tr>
      <td class="fill-default">17</td>
      <td class="fill-default">18</td>
      <td class="fill-default">19</td>
      <td class="fill-default">20</td>
      <td class="fill-default">21</td>
    </tr>
  </table>
  
  <!-- SVG to draw the updated continuous arrow path -->
  <svg>
    <path 
      d="M 35 340 L 340 340 L 340 35 M 325 50 L 340 35 L 355 50" 
      stroke="#0b57d0" 
      stroke-width="8" 
      fill="none" 
      stroke-linecap="round" 
      stroke-linejoin="round" 
    />
  </svg>
</div>

</body>
</html>

The numbers are filled in method 1 if the dimentions of the blue filled square is odd and method 2 if the dimentions of the blue filled square are even.
For simplicity, we will be dealing with $x \geq y$ here. You can easily derive the result for the other case as well. 
If the cells were filled using method 1, we have already filled the square of dimentions $(x-1)^2$ and there are $y$ extra numbers that are entered. 
If the cells were filled using method 2, we have already filled the square of dimentions $(x-1)^2$ and there are $x + (x - y) = 2x - y$ extra numbers that are entered. 

## Code
``` cpp
#include <bits/stdc++.h>
using namespace std;

int32_t main(){
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    long long t;
    cin >> t;
    while(t--){
        long long x, y;
        cin >> x >> y;
        if(x > y){
            if(x%2 == 1){
                cout << ((x-1)*(x-1) + y);
            }
            else{
                cout << ((x-1)*(x-1) + 2*x - y);
            }
        }
        else{
            if(y%2 == 0){
                cout << ((y-1)*(y-1) + x);
            }
            else{
                cout << ((y-1)*(y-1) + 2*y - x);
            }
        }
        cout<<"\n";
    }
}
```