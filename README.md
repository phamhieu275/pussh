pussh
=====

Parallel SSH, batch and command line oriented.

Basic commands, where output is send prefixed by the host name :

    $ pussh -h host1,host2bis,... uname -a
    host1   : Linux host1 3.2.0-4-amd64 #1 SMP Debian 3.2.41-2 x86_64 GNU/Linux
    host2bis: Linux host2 3.2.0-3-kirkwood #1 Mon Jul 23 22:36:47 UTC 2012 armv5tel GNU/Linux
    ...

You can fetch the host list from a file or from stdin :

    $ pussh -f servers uname -a
    $ fetch-servers | pussh -f - uname -a

Use it with pipes as you would normally, for instance to sort server by rootfs usage :

    $ pussh -f servers df -h / |grep /dev |sort -rn -k5

Or collect per-host output, like for inventories :

    $ pussh -f servers -o %h-hw.txt lshw

And you can even pipe each SSH session to a specific command (%h is also available if you
whish) :

    $ pussh -f servers -o '|grep feature' command >output

Obviously, you have the same features for the SSH sessions input :

    $ pussh -f servers -i file command
    $ pussh -f servers -i %h.data command
    $ pussh -f servers -i 'get-data %h|' command

You may mix -i and -o freely.

More interestingly, you can send and execute a command in one go (but it won't handle
the dependencies for you, make sur your executable is self-contained or your host
environment is compatible with the target ones) :

    $ pussh -f servers -u my-deploy.sh args ...

Most of the time the total time of execution is limite by the SSH connection
establishement rate, and this rate is itself limited by the SSH agent. It is
recommand to tune your rate one for a specific host and stick to it. Exceeding
rates generate strange connection errors.

   $ alias pussh='pussh -r 50'


History
=======

Pussh has been an internal tool at Bearstech since roughly 2008. It started with
4 or 5 lines of shell and quickly evolved to its current form. It's still heavily
used on a 200+ cluster server, and most of the time reach all of its targets in
a few seconds. It is coupled to a cloud management solutions to generate a
useful list of hosts from very simple descriptions.

It has only been recently modified as a generic project, we had to remove
several hardcoded thingies. And add a real doc. And some tests. Actually
we had to make it much better, which is cool.
