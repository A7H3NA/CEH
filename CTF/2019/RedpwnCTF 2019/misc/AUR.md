# RedpwnCTF 2019 - Writeup (misc:AUR)

A team that used the summer vacation period entered RedpwnCTF 2019.
Since I solved 6 questions (+1 question), writeup is written only as much as possible.


````
Written by: arinerron
Help! We lost aur flag in the Arch User Repository! Go find it for us.
````


The security distribution Arch Linux has its own repository.
[https://wiki.archlinux.jp/index.php/Arch_User_Repository:embed:cite]


Try accessing AUR (Arch User Repository).
[https://aur.archlinux.org/:embed:cite]


Search the flag in the AUR home from the question sentence.
Since Written by: arinerron, check Mr.arinerron's post from [Search Criteria].
It seems that about 4 are registered.
[https://aur.archlinux.org/packages/?O=0&SeB=s&K=arinerron&outdated=&SB=n&SO=a&PP=50&do_Search=Go:embed:cite]


Among them, when you look at game-git.git, flag is added to [View Changes] of [Package Actions] one day ago.
Check the added flag posts.
[https://aur.archlinux.org/cgit/aur.git/commit/?h=game-git:embed:cite]


###### flag{w0w-have_fun_in-g4m3!}
