## 01. Hartree–Fock法による計算(1)
- Hartree–Fock法（(R)HF/6-31G(d)）で構造最適化
```
>> Do While

&SEWARD

&SCF

&SLAPAF

>> End Do
```

によって reactant, ts, product の構造最適化を行う

それぞれ、-229.38262545, -229.27063299, -229.27089982 になればいい

reactant: -229.3826254492

ts: -229.2706329884

product: -229.2708998244

- 1 hartree = 627.51 kcal/mol によってエネルギー変換

## 02. Hartree–Fock法による計算(2)
```
&MCKINLEY
```
によってギブスエネルギーを得る

-229.334576, -229.227300, -229.226784 になればいい

reactant: -229.334575

ts: -229.227300

product: -229.226784 

より、正しい値が得られた！！

## 03. CASSCFによる計算(1)
casscfで活性空間指定して計算 reactant, ts, product

-229.57067459, -229.51991905, -229.52647146 になればいいらしい

![alt text](<スクリーンショット 2024-07-11 032557.png>)
<center>reactantの活性空間</center>

![alt text](<スクリーンショット 2024-07-11 032319.png>)
<center>productの活性空間</center>


reactant: -229.57067374

product: -229.52647142

ts: -229.51991952

より正しく計算されてる！

## 04. CASSCFによる計算(2)
symmetry 指定して計算　何を言っているのか全く分からない
波動関数の対称性
つまり、スレーター行列式であらわされる波動関数はag*ag*au*b1*...みたいになってる
これの積がag(全対称)になるようにデフォオルトではなってる
ふつう2電子占有されているとL*L=ag となるので基本的には全対称表現の構造が最安定
だが、鉄原子などはd6で最安定構造は2電子占有ではない
よってsymmetryの設定で最安定を見つけることが必要

productのsymmetryを操作

symmetry 1: -229.52647142,ag h,l

symmetry 2: -229.32464124,b3u h

symmetry 3: -229.32887543,b2u h,l
これの3重項が二番目に安定

symmetry 4: -229.28383670,b1g l

symmetry 5: -229.33513755,b1u h,l

symmetry 6: -229.38287510,b2g h

symmetry 7: -229.37121003,b3g h,l

symmetry 8: -229.33671464,au l

エネルギーの大小関係は以下のようになった

1 << 6 < 7 ...

::    RASSCF root number  1 Total energy:   -229.53800875
::    RASSCF root number  1 Total energy:   -229.37838269
::    RASSCF root number  1 Total energy:   -229.34671906
::    RASSCF root number  1 Total energy:   -229.34991101
::    RASSCF root number  1 Total energy:   -229.33788971
::    RASSCF root number  1 Total energy:   -229.52946667
::    RASSCF root number  1 Total energy:   -229.30765874
::    RASSCF root number  1 Total energy:   -229.35570907
::    RASSCF root number  1 Total energy:   -229.37391980
::    RASSCF root number  1 Total energy:   -229.40095323
::    RASSCF root number  1 Total energy:   -229.39123796
::    RASSCF root number  1 Total energy:   -229.38689021
::    RASSCF root number  1 Total energy:   -229.39847747
::    RASSCF root number  1 Total energy:   -229.42727670
::    RASSCF root number  1 Total energy:   -229.36367214
::    RASSCF root number  1 Total energy:   -229.36015536


## 05. CASSCFによる計算(3)
- reactant: -229.57428994, product : -229.53800875 が構造最適化によって得られるとうれしい

reactant: -229.57428966 gibbs: -229.532359

product: -229.53800875 gibbs: -229.487939

が得られた。　あってる！

ギブスエネルギーも記録しとく

## 06. CASSCFによる計算(4)

構造最適化してないので意味ないかもだけど計算結果は

energy: -229.50916460

gibbs: -229.467105

が得られた

1つだけ勾配がマイナスになっているか確認

-780.77　の基準振動を発見

確かに一つだけマイナスの基準振動がある！
![alt text](<スクリーンショット 2024-07-11 012853.png>)
<center>見つけた負の基準振動</center>

## 07. CASSCFによる計算(5)
対称性をc2vにする

ts_sp.freq.moldenを確認すると、確かに一つだけ-662になってる

だが、振動部の結合が二重結合になってる

単結合では？　まあどっちでもいいか…
![alt text](<スクリーンショット 2024-07-11 021102.png>)

ts energy -229.50902959

ts gibbs energy -229.463788

となればよい

ts: -229.50902959

ts gibbs: -229.463789

うまくいってる！！

||CASSCF energy in a.u.|$`\Delta E`$ in kcal mol<sup>–1</sup>|CASSCF Gibbs free energy in a.u.|$`\Delta G`$ in kcal mol<sup>–1</sup>|
|-|-|-|-|-|
|reactant|-229.57428994|0.00|-229.532359|0.00|
|TS|-229.50902959|+40.95|-229.463788|+43.03|
|product|-229.53800875|+22.77|-229.487939|+27.87|



## 08. CASSCFによる計算(6)
いよいよ終盤

tsをircに沿って動かしてproduct,reactantになるか確認

reactantのエネルギー-229.57428966,productのエネルギー-229.53800875

に近いかどうか確認

多少ずれるらしい

-229.57424533 と -229.53770926 が得られた

以上より計算結果はproduct,reactant間のtsであることを表している！！

## 09. CASPT2による計算