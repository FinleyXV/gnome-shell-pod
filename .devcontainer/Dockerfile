FROM archlinux:latest

# Install required packages
RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm \
      gnome-session \
      gnome-shell \
      gnome-terminal \
      xorg-xinit \
      xorg-server-xvfb \
      xdotool \
      xautomation \
      sudo \
      systemd \
      dbus \
      polkit && \
    pacman -Scc --noconfirm

# Copy system configuration (make sure you have etc/ folder in your build context)
COPY etc /etc

# Enable and configure systemd services and user
RUN systemctl unmask systemd-logind.service console-getty.service getty.target && \
    systemctl enable xvfb@:99.service && \
    systemctl set-default multi-user.target && \
    useradd -m -G users,adm gnomeshell && \
    echo "gnomeshell ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Copy your scripts to /usr/local/bin
COPY bin /usr/local/bin

# Set default command to start systemd with the necessary cgroup setting
CMD ["/usr/lib/systemd/systemd", "systemd.unified_cgroup_hierarchy=0"]
