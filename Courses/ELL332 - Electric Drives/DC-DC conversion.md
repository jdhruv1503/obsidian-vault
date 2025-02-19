## Chopper

$V_{out} = DV_{in}$

## Principles

Volt-second balance: Integral Vdt is zero
Amp-second balance: Integral idt is zero

## Buck

![[Pasted image 20250219082303.png]]

![[Pasted image 20250219082319.png]]

$V_{out} = DV_{in}$
$\Delta i_L = \frac{V_o}{L}$
$D_{min} = \frac{V_{o}}{V_{in max}}$
$L = \frac{V_o(1-D_{min})}{\Delta i_L f_s}$
$C = \frac{\Delta i_L}{8\Delta V_o f_s}$

## Boost

![[Pasted image 20250219082758.png]]

![[Pasted image 20250219082810.png]]

$V_o = \frac{V_{in}}{1-D}$
$\Delta i_L = \frac{V_{in}}{L}$
$D_{min}$ in the same way with $D_{max}$ too
$L = \frac{V_{imax}D_{min}}{\Delta i_L f_s}$
$C = \frac{i_o D}{\Delta V_o f_s}$

## Buck Boost Converter

![[Pasted image 20250219083251.png]]

![[Pasted image 20250219083301.png]]

$V_o = \frac{-DV_{in}}{1-D}$
$\Delta i_L = \frac{V_{in}}{L}$
$D_{min}$ in the same way with $D_{max}$ too
$L = \frac{V_{o}(1-D_{min})}{\Delta i_L f_s}$
$C = \frac{i_o D}{\Delta V_o f_s}$