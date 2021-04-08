# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: WE01
### Category: Web (Easy)

## Description:
#### [100 pts]: View the page at [https://cfta-we01.allyourbases.co](https://cfta-we01.allyourbases.co) and try to get the flag.

## Walkthrough:

Navigating to the provided URL we're presented with the following page:

![](WE01%20Writeup.001.png)

This looks really strange. It reminded me of [JSFuck](http://www.jsfuck.com/), an esoteric programming style in Javascript, but it obviously has different symbols. So I copied the text and googled it to see if I could find anything on it. 

The first thing that appeared was this [tweet](https://twitter.com/aemkei/status/755147932081483776) which mentioned [Aurebesh.js](http://aem1k.com/aurebesh.js/). This is a cool tool that will generate valid javascript code using symbols/letters from other languages, including a writing system from Stars Wars called [Aurebesh](https://starwars.fandom.com/wiki/Aurebesh).

So, since this is valid javascript we can just run the code in the console of a browser:

```
>> ロ='',コ=!ロ+ロ,Y=!コ+ロ,ㅣ=ロ+{},ᗐ=コ[ロ++],Ξ=コ[Δ=ロ],ᐳ=++Δ+ロ,ㅡ=ㅣ[Δ+ᐳ],ウ="+=*:.",コ[ㅡ+=ㅣ[ロ]+(コ.Y+ㅣ)[ロ]+Y[ᐳ]+ᗐ+Ξ+コ[Δ]+ㅡ+ᗐ+ㅣ[ロ]+Ξ][ㅡ](ㅣ[Δ+ᐳ]+ㅣ[ロ]+(コ.Y+ㅣ)[ロ]+Y[ᐳ]+ㅣ[ロ]+Y[Δ]+コ[ᐳ]+ウ[ᐳ+ロ]+Y[Δ]+ㅣ[ロ]+(([]+([]+[])[ㅡ])[ᐳ*(ᐳ+ロ)+Δ])+"(Y[Δ-Δ]+Y[Δ]+Y[ロ]+(([]+([]+[])[ㅡ])[ᐳ*(ᐳ+ロ)+Δ])+ウ[ᐳ]+ㅣ[(ᐳ+ロ)*ロ+ᐳ]+コ[Δ]+(コ.Y+ㅣ)[ロ]+(コ.Y+ㅣ)[ᐳ*Δ-ロ]+ㅡ[ロ-ロ]+ㅡ[ロ]+(コ.Y+ㅣ)[ᐳ-ロ]+コ[ᐳ]+ウ[ロ-ロ]+ㅣ[ロ]+ㅣ[Δ]+Y[Δ-Δ]+コ[Δ]+Y[ᐳ]+ㅡ[ᐳ-ᐳ]+Y[ロ]+ᗐ+(コ.Y+ㅣ)[ᐳ*Δ-ロ]+ㅣ[ロ]+(コ.Y+ㅣ)[ロ]+ウ[ロ]+ㅣ[ᐳ]+Y[ᐳ]+ウ[Δ]+Y[Δ-Δ]+コ[Δ]+(コ.Y+ㅣ)[ロ] )")()
flag: unicode+obfuscation=js*fun
```

### Flag: unicode+obfuscation=js*fun