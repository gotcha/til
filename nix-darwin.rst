Setup Nix-Darwin
----------------

2023, Oct 11th

With Nix installed via Determinate Systems installer.

Run this as stated by nix-darwin README::

    mkdir -p ~/.config/nix-darwin
    cd ~/.config/nix-darwin
    nix flake init -t nix-darwin
    sed -i '' "s/simple/$(scutil --get LocalHostName)/" flake.nix

Run this the first time, when nix darwin has not been installed::

    nix run nix-darwin -- switch --flake ~/.config/nix-darwin

This asked for my user password for sudo access in order to edit machine files.

Then complained because ``/etc/zshrc``
and ``/etc/nix/nix.conf`` already existed and asked me to rename them::

    sudo mv /etc/zshrc /etc/zshrc.before-nix-darwin
    sudo mv /etc/nix/nix.conf /etc/nix/nix.conf.before-nix-darwin

When I tried to run again ``nix run nix-darwin`` as previous attempt had failed, it also failed as renaming ``/etc/nix/nix.conf`` was not there anymore to allow ``nix-command`` and ``flakes`` support. So I had to run::

    nix run nix-darwin --extra-experimental-features nix-command --extra-experimental-features flakes -- switch --flake ~/.config/nix-darwin

Which failed again because I had not set the proper architecture in the original ``~/.config/nix-darwin``.
After switching from ``x86_64-darwin`` to ``aarch64-darwin``, I could run this again::   

    nix run nix-darwin --extra-experimental-features nix-command --extra-experimental-features flakes -- switch --flake ~/.config/nix-darwin

And success !
