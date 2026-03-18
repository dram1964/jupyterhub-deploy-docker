FROM quay.io/jupyter/datascience-notebook

ARG JUPYTER_WORKDIR

USER root

# Generic Linux packages
RUN <<EOF
apt-get update --yes
apt-get upgrade --yes
apt-get install --yes --no-install-recommends gnupg2
EOF

# ODBC PostgreSQL drivers
RUN <<EOF
apt-get install --yes libodbcinst2 unixodbc unixodbc-dev odbcinst odbc-postgresql
sed -i -r 's/=(psqlodbc(a|w).so|libodbcpsqlS.so)/=\/usr\/lib\/x86_64-linux-gnu\/odbc\/\1/' /etc/odbcinst.ini
EOF

# TDS ODBC drivers
RUN <<EOF
apt-get install --yes --no-install-recommends freetds-dev freetds-bin tdsodbc
EOF

# Microsoft SQL ODBC drivers
RUN <<EOF
curl -sSL -O https://packages.microsoft.com/config/ubuntu/$(grep VERSION_ID /etc/os-release | cut -d '"' -f 2)/packages-microsoft-prod.deb
dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
apt-get update
ACCEPT_EULA=Y apt-get install --yes msodbcsql18
export PATH="$PATH:/opt/mssql-tools18/bin"
EOF

# Clear packages not required during runtime
RUN <<EOF
apt-get purge --yes gnupg2
apt-get autoremove --yes
apt-get clean --yes
rm -rf /var/lib/apt/lists/*
EOF

###############################################################################
# Conda, Python, R packages                                                   #
###############################################################################

# Install packages as final user (prevents ~/.cache/conda to be owned by root)
USER ${NB_UID}

# Upgrade Conda
RUN <<EOF
conda update --yes conda
conda update --all --yes
EOF

RUN conda install --quiet --yes psycopg2 pyodbc pymssql

# R minimum general-use dependencies
RUN conda install --quiet --y --channel r \
      r-bit64 \
      r-here \
      r-tidyverse \
      r-config \
      r-odbc

# Clean conda after install
RUN conda clean --all --force-pkgs-dirs --y

# Reset user to root to continue setup
USER root

###############################################################################
# Prepare working directory                                                   #
###############################################################################

RUN mkdir -p ${JUPYTER_WORKDIR}
RUN chown -R ${NB_UID}:${NB_GID} ${JUPYTER_WORKDIR}

# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}
WORKDIR ${JUPYTER_WORKDIR}