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

<style>
.number-grid {
  display: grid;
  grid-template-columns: repeat(5, 70px);
  gap: 4px;
  background: #1a1a1a;
  border: 4px solid #1a1a1a;
  width: max-content;
  margin: 1rem auto;
}

.number-grid i {
  width: 100%;
  height: 70px;
  display: grid;
  place-items: center;
  background: #dcdcdc;
  color: #000000;
  font-style: normal;
  font-family: Arial, sans-serif;
  font-weight: bold;
  font-size: 24px;
}

.number-grid .blue {
  background: #1E90FF;
}
</style>

<div class="number-grid">
  <i class="blue">1</i><i class="blue">2</i><i class="blue">9</i><i>10</i><i>25</i>
  <i class="blue">4</i><i class="blue">3</i><i class="blue">8</i><i>11</i><i>24</i>
  <i class="blue">5</i><i class="blue">6</i><i class="blue">7</i><i>12</i><i>23</i>
  <i>16</i><i>15</i><i>14</i><i>13</i><i>22</i>
  <i>17</i><i>18</i><i>19</i><i>20</i><i>21</i>
</div>

## Solution
Now let us divide the grid into squares starting at the top left corner of the grid. 
Now I pick a cell (x,y) outside a square of dimensions $a \times a$. I know the value on cell (x,y) has to be greater than $a^2$. This is because all the numbers from $1$ to $a^2$ are already present in the square. 
So how do we use this to solve this problem?
Let me consider the largest square the cell (x,y) is not part of. What should be the dimensions of this square? If $x \geq y$ this square must have dimensions $(x-1) \times (x-1)$. If $y \geq x$ this square must have dimensions $(y-1) \times (y-1)$. 
After we fill the square, we need to find how the remaining numbers are filled. These numbers are filled using method 1 and method 2 alternatively.

<div class="spiral-wrapper">
  <div class="spiral-grid">
    <div class="fill-blue">1</div><div class="fill-blue">2</div><div class="fill-blue">9</div><div class="fill-default">10</div><div class="fill-default">25</div>
    <div class="fill-blue">4</div><div class="fill-blue">3</div><div class="fill-blue">8</div><div class="fill-default">11</div><div class="fill-default">24</div>
    <div class="fill-blue">5</div><div class="fill-blue">6</div><div class="fill-blue">7</div><div class="fill-default">12</div><div class="fill-default">23</div>
    <div class="fill-default">16</div><div class="fill-default">15</div><div class="fill-default">14</div><div class="fill-default">13</div><div class="fill-default">22</div>
    <div class="fill-default">17</div><div class="fill-default">18</div><div class="fill-default">19</div><div class="fill-default">20</div><div class="fill-default">21</div>
  </div>
  <svg class="spiral-svg">
    <path d="M 255 20 L 255 255 L 35 255 M 55 240 L 35 255 L 55 270" stroke="#0b57d0" stroke-width="8" fill="none" stroke-linecap="round" stroke-linejoin="round" />
  </svg>
</div>

<div class="spiral-wrapper">
  <div class="spiral-grid">
    <div class="fill-blue">1</div><div class="fill-blue">2</div><div class="fill-blue">9</div><div class="fill-blue">10</div><div class="fill-default">25</div>
    <div class="fill-blue">4</div><div class="fill-blue">3</div><div class="fill-blue">8</div><div class="fill-blue">11</div><div class="fill-default">24</div>
    <div class="fill-blue">5</div><div class="fill-blue">6</div><div class="fill-blue">7</div><div class="fill-blue">12</div><div class="fill-default">23</div>
    <div class="fill-blue">16</div><div class="fill-blue">15</div><div class="fill-blue">14</div><div class="fill-blue">13</div><div class="fill-default">22</div>
    <div class="fill-default">17</div><div class="fill-default">18</div><div class="fill-default">19</div><div class="fill-default">20</div><div class="fill-default">21</div>
  </div>
  <svg class="spiral-svg">
    <path d="M 35 340 L 340 340 L 340 35 M 325 50 L 340 35 L 355 50" stroke="#0b57d0" stroke-width="8" fill="none" stroke-linecap="round" stroke-linejoin="round" />
  </svg>
</div>

The numbers are filled in method 1 if the dimensions of the blue filled square is odd and method 2 if the dimensions of the blue filled square are even.
For simplicity, we will be dealing with $x \geq y$ here. You can easily derive the result for the other case as well. 
If the cells were filled using method 1, we have already filled the square of dimensions $(x-1)^2$ and there are $y$ extra numbers that are entered. 
If the cells were filled using method 2, we have already filled the square of dimensions $(x-1)^2$ and there are $x + (x - y) = 2x - y$ extra numbers that are entered. 

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
} ```