## Manually todo
in cydia
* openssh
* aptbackup

----------------------
## Basic setup
ln -s /private/var/local /usr

apt-get install screen vim

apt-get install '.*cmds.*' less sudo wget

apt-get install com.innoying.sbutils coreutils inetutils

apt-get install git subversion

apt-get install rsync com.was.mysql

apt-get install sbsettings 

dpkg -i archives/KeepAlive.deb

----------------------
## Install Perl
wget -cO /tmp/coredev.pub http://coredev.nl/cydia/coredev.pub

apt-key add /tmp/coredev.pub

echo 'deb http://coredev.nl/cydia iphone main' > /etc/apt/sources.list.d/coredev.nl.list 

apt-get update

apt-get install perl

apt-get install p5-plack* p5-Dist-Zilla* p5-dbix-class*

apt-get install p5-dbd-sqlite

apt-get install p5-xml-*

apt-get install p5-*psgi*

apt-get install p5-ev

apt-get install p5-HTTP-Parser-XS

apt-get install p5-*fcgi*

apt-get install p5-Devel-StackTrace-WithLexicals

apt-get install p5-Net-Server*

apt-get install p5-Proc-Wait3

cd /usr/local/lib/perl5/5.10.0/arm-iphoneos/CORE/

wget http://src.gnu-darwin.org/src/lib/libutil/libutil.h

----------------------
## Install C and header files for CPAN install
wget http://gammalevel.com/forever/fake-libgcc_1.0_iphoneos-arm.deb

dpkg -i fake-libgcc_1.0_iphoneos-arm.deb

wget http://apt.saurik.com/debs/libgcc_4.2-20080410-1-6_iphoneos-arm.deb

dpkg -i libgcc_4.2-20080410-1-6_iphoneos-arm.deb 

apt-get install iphone-gcc libsigc++

apt-get install ldid zip unzip patch gawk

ln -s /usr/bin/gcc /usr/bin/cc

apt-get install make automake

mkdir /usr/local/include

apt-get install com.bigboss.20toolchain

rsync -Pa iPhoneOS6.0.sdk/usr/lib /usr/lib

---------------------
## Reference
http://www.mobile01.com/topicdetail.php?f=591&t=3030583

http://shyunsei.9ten.net/?p=472

---------------------
## Initial Global Bash Env
Copy and Paste these lines to /etc/profile

export C_INCLUDE_PATH=/var/include

export CPLUS_INCLUDE_PATH=/var/include

export OBJC_INCLUDE_PATH=/var/include

export CPLUS_INCLUDE_PATH=/private/var/include/c++/4.0.0:/private/var/include/c++/4.0.0/i686-apple-darwin9

export CPPFLAGS=-I/usr/local/include/

export CPP=/usr/bin/cpp

export PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\] [PWD: \w]\n> '

. $HOME/.bashrc

---------------------
## Initial User Bash Env
Copy and Paste these lines to ~mobile/.bashrc

export PERL_LOCAL_LIB_ROOT="/var/mobile/perl5";

export PERL_MB_OPT="--install_base /var/mobile/perl5";

export PERL_MM_OPT="INSTALL_BASE=/var/mobile/perl5";

export PERL5LIB=".:lib:/var/mobile/perl5/lib/perl5/arm-iphoneos:/var/mobile/perl5/lib/perl5";

export PATH=".:bin:/var/mobile/perl5/bin:$PATH";

---------------------
## Add hot key "gf" to open module file on the namespace
Copy and Paste these lines to ~mobile/.vimrc

function! GetCursorModuleName(fname)

    let cw = substitute( a:fname, '.\{-}\(\(\w\+\)\(::\w\+\)*\).*$', '\1', '' )

    return cw

endfunction

function! TranslateModuleName(n)

    return substitute( a:n, '::', '/', 'g' ) . '.pm'

endfunction

function! GetPerlLibPaths()

    let out = system('/usr/bin/env perl -e ''print join "\n", @INC''')

    let paths = split( out, "\n" )

    return paths

endfunction

function! FindModuleFileInPaths(file)

    let paths = [ 'lib' ] + GetPerlLibPaths()

    let fname = TranslateModuleName(GetCursorModuleName(a:file))

    for p in paths

        let f = p . '/' . fname

        if filereadable(f)

            return f

        endif

    endfor

    echo "File not found: " . fname

endfunction

set includeexpr=FindModuleFileInPaths(v:fname)

set isfname+=:

--------------------------
## Install cpanm and local::lib
cd $HOME

wget -cO test http://search.cpan.org/src/MIYAGAWA/App-cpanminus-1.5021/bin/cpanm

perl test --local-lib=$HOME/perl5 local::lib App::cpanminus

rm test

cpanm DBD::mysql -v

--------------------------
## Install Git

cd git-1.7.9.2-ready-make-install-for-ios

make NO_TCLTK=1 prefix=/usr/local install

git --version
