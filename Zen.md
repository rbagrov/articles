
# Zen of Python

Very recently a guy posted the zen of python on linkedin, which is great, but ohnestly got me thinking where it got from and took a look under the hood.

First, little history. Zen of Python was written by [Tim Peters](https://stackoverflow.com/users/2705542/tim-peters) in 2004 under document called [PEP20](https://www.python.org/dev/peps/pep-0020/). It represents guiding principles and aphorisms for writting good Python code. So these principles should have been 20 but he wrote only 19. People say he intentionally left one out so people can add their own bit to complete their Zen.

Here is how you can see the Zen:

```python
In [1]: import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

Pretty cool, right. Yeah, but if the guy said 20, wrote 19 in a document that doesn't need to have consistent index, tells me there is more to it, so I tool a look where that file is in my system

```python
In [2]: this.__file__
Out[2]: '/home/rbagrov/anaconda3/lib/python3.5/this.py'
```

Very nice. Lets see if there are more easter eggs in it...

```python
s = """Gur Mra bs Clguba, ol Gvz Crgref
 
 Ornhgvshy vf orggre guna htyl.
 Rkcyvpvg vf orggre guna vzcyvpvg.
 Fvzcyr vf orggre guna pbzcyrk.
 Pbzcyrk vf orggre guna pbzcyvpngrq.
 Syng vf orggre guna arfgrq.
 Fcnefr vf orggre guna qrafr.
 Ernqnovyvgl pbhagf.
 Fcrpvny pnfrf nera'g fcrpvny rabhtu gb oernx gur ehyrf.
 Nygubhtu cenpgvpnyvgl orngf chevgl.
 Reebef fubhyq arire cnff fvyragyl.
 Hayrff rkcyvpvgyl fvyraprq.
 Va gur snpr bs nzovthvgl, ershfr gur grzcgngvba gb thrff.
 Gurer fubhyq or bar-- naq cersrenoyl bayl bar --boivbhf jnl gb qb vg.
 Nygubhtu gung jnl znl abg or boivbhf ng svefg hayrff lbh'er Qhgpu.
 Abj vf orggre guna arire.
 Nygubhtu arire vf bsgra orggre guna *evtug* abj.
 Vs gur vzcyrzragngvba vf uneq gb rkcynva, vg'f n onq vqrn.
 Vs gur vzcyrzragngvba vf rnfl gb rkcynva, vg znl or n tbbq vqrn.
 Anzrfcnprf ner bar ubaxvat terng vqrn -- yrg'f qb zber bs gubfr!"""
 
 d = {}
 for c in (65, 97):
     for i in range(26):
         d[chr(i+c)] = chr((i+13) % 26 + c)
 
 print("".join([d.get(c, c) for c in s]))
```

Wait what is the string all about. It is coded and obviously below is the decoder.

Well boys and girls this is the famous [ROT13](https://en.wikipedia.org/wiki/ROT13). Part of famous ROT codecs where they rotate by X places. In our case it is 13 places.
But there must be more pythonic way to do it. Sure there is. Here is how:

```python
In [1]: import this
In [2]: import codecs
In [3]: this.s
Out[3]: "Gur Mra bs Clguba, ol Gvz Crgref\n\nOrnhgvshy vf orggre guna htyl.\nRkcyvpvg vf orggre guna vzcyvpvg.\nFvzcyr vf orggre guna pbzcyrk.\nPbzcyrk vf orggre guna pbzcyvpngrq.\nSyng vf orggre guna arfgrq.\nFcnefr vf orggre guna qrafr.\nErnqnovyvgl pbhagf.\nFcrpvny pnfrf nera'g fcrpvny rabhtu gb oernx gur ehyrf.\nNygubhtu cenpgvpnyvgl orngf chevgl.\nReebef fubhyq arire cnff fvyragyl.\nHayrff rkcyvpvgyl fvyraprq.\nVa gur snpr bs nzovthvgl, ershfr gur grzcgngvba gb thrff.\nGurer fubhyq or bar-- naq cersrenoyl bayl bar --boivbhf jnl gb qb vg.\nNygubhtu gung jnl znl abg or boivbhf ng svefg hayrff lbh'er Qhgpu.\nAbj vf orggre guna arire.\nNygubhtu arire vf bsgra orggre guna *evtug* abj.\nVs gur vzcyrzragngvba vf uneq gb rkcynva, vg'f n onq vqrn.\nVs gur vzcyrzragngvba vf rnfl gb rkcynva, vg znl or n tbbq vqrn.\nAnzrfcnprf ner bar ubaxvat terng vqrn -- yrg'f qb zber bs gubfr!"

In [4]: codecs.decode(this.s, 'rot_13')
Out[4]: "The Zen of Python, by Tim Peters\n\nBeautiful is better than ugly.\nExplicit is better than implicit.\nSimple is better than complex.\nComplex is better than complicated.\nFlat is better than nested.\nSparse is better than dense.\nReadability counts.\nSpecial cases aren't special enough to break the rules.\nAlthough practicality beats purity.\nErrors should never pass silently.\nUnless explicitly silenced.\nIn the face of ambiguity, refuse the temptation to guess.\nThere should be one-- and preferably only one --obvious way to do it.\nAlthough that way may not be obvious at first unless you're Dutch.\nNow is better than never.\nAlthough never is often better than *right* now.\nIf the implementation is hard to explain, it's a bad idea.\nIf the implementation is easy to explain, it may be a good idea.\nNamespaces are one honking great idea -- let's do more of those!"

```

Boom. Nice.

Of course come to think there are many ways to do it, since it just a matter of substitution of characters.
So there you go. Now you know more about Zen of Python.
