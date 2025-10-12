Use the `ls` command to see the contents of the home directory. We can see that there's a `data.txt` file.
```console
bandit9@bandit:~$ ls
data.txt
```
If we read that text file we will see the following:
```console
q<CE>^R*<9F>P<C5><D5>+}<DE>Ż<AD>oV<E2><DF>^Y^KĐ<CB>/<B8><E6>w<E6><89>K$<87><9B><92>a<B5><84><88>q.9,<F4><86>~<E8>^\}<F3><AD>^?<AE><F0>+<E4>^U-s<FF>4^L<96><DA>=<A1>]^E^\x<FA>{^]ESC<B0>^^<99>^E|<D6>qI<F2>)<85><<EB><A1><F5><AE><AF>^U^W;<C5><DB<DB>ȏ<D0>Y<D4>^D<FA><DC>^S<92><9F><EB>w<8A><88>[<E7>fs{<D6><<B6>|^@<B3>M<EB>^C<CD>^v<AE>$<DE>^]ъ^^<AD>^\<DF>mY>m{<97>=<9<98>bJ<97><9F><B4><96><F4><B1><B5>x<B3>Z<91><A5><<CE><EF>^F<E4><94>N<E4><89>75<F9>~<E3>4<F4> <B3>y<9A>K^]?<EF>A<A1><8B>]ESC<90><CA>N<BB><9D><FA><B4>^MԀ2<C1>^K<97><E8><AF><DB>Y^\<83>`?<F3>a<89>.'^Gf{W<DE>*<81>>^Ch<A0>~奸<C8>:<CF><DE>^U<A9>c׶<FC><CB>w^^nb<E7>?ӫ<B2>3
```
That looks like gibberish. The description of the level wants us to find the string that is preceded by a few `=` characters.
So we can use the `strings` command to filter out all the non-printable characters.
```console
bandit9@bandit:~$ strings data.txt
q.9,
mY>m{
/<[;
fL{[
,)wp
fQ4U_
U       [^
vy%-
,[(P
.
.
.
Fb=G
7h08
@udz
9V\N
F%?TI0oG
AUE:F
```
Now we will use the `grep` command to filter out the strings that are preceded by `=` characters.
```console
bandit9@bandit:~$ strings data.txt | grep "=="
========== the
========== password
Q========== is%
>u`9J========== [REDACTED]
```
And there we have it. We found the password for the next level.
