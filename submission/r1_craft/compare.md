# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_060000.jpg
- L5 center SPURIOUS
- R5 mid MISSING
- R7 center MISSING
- R9 center MISSING
## adasind_086220.jpg
## adasind_102750.jpg
- L1+R1 mid ATTRIBUTE
- L4+R2 edge WRONG_CLASS
- L5 center SPURIOUS
- L6+R5 center WRONG_CLASS

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 9 | 6 | 3 | 3 |
| mid | 8 | 7 | 1 | 0 |
| edge | 3 | 2 | 1 | 1 |
