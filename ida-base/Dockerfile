FROM thawsystems/wine-stable
MAINTAINER David Manouchehri

USER root

# RUN apt-get -y update && \
#	DEBIAN_FRONTEND=noninteractive apt-get -y install wget

RUN printf '#!/usr/bin/env bash\nWINEDEBUG=-all exec wine "/home/ida/idal.exe" "$@"\n' > /usr/local/bin/idal && \
	printf '#!/usr/bin/env bash\nWINEDEBUG=-all exec wine "/home/ida/idal64.exe" "$@"\n' > /usr/local/bin/idal64 && \
	chmod +x /usr/local/bin/idal /usr/local/bin/idal64

RUN useradd -m ida
USER ida
WORKDIR /home/ida
ENV HOME /home/ida

RUN wget "https://www.python.org/ftp/python/2.7.13/python-2.7.13.msi" "https://www.python.org/ftp/python/2.7.13/python-2.7.13.msi.asc" && \
	curl https://keybase.io/stevedower/pgp_keys.asc | gpg --import && \
	# gpg --recv-keys 7ED10B6531D7C8E1BC296021FC624643487034E5 && \
	gpg --verify python-2.7.13.msi.asc && \
	msiexec /i "python-2.7.13.msi" /passive /norestart ADDLOCAL=ALL InstallAllUsers=1 && \
	rm python-2.7.13.msi python-2.7.13.msi.asc
