FROM thawsystems/wine-stable
MAINTAINER David Manouchehri

USER root

COPY ida-service/flask_service.py /usr/local/bin/
COPY ida-service/ida_service.sh /usr/local/bin/

RUN apt-get -y update && \
	DEBIAN_FRONTEND=noninteractive apt-get -y install wget curl python-pip && \
	wget -q "https://raw.githubusercontent.com/thawsystems/docker-ida/master/ida-base/requirements-linux.txt" && \
	python -m pip install -U pip setuptools && \
	python -m pip install -r requirements-linux.txt && \
	rm -vf requirements-linux.txt && \
	printf '#!/usr/bin/env bash\nWINEDEBUG=-all exec wine "/home/ida/.wine/drive_c/ida/idaw.exe" "$@"\n' > /usr/local/bin/idaw && \
	printf '#!/usr/bin/env bash\nWINEDEBUG=-all exec wine "/home/ida/.wine/drive_c/ida/idaw64.exe" "$@"\n' > /usr/local/bin/idaw64 && \
	chmod +x /usr/local/bin/idaw /usr/local/bin/idaw64 \
		/usr/local/bin/flask_service.py /usr/local/bin/ida_service.sh && \
	useradd -m ida && mkdir /shared && chown ida:ida /shared

USER ida
WORKDIR /home/ida
ENV HOME /home/ida

RUN wget -q "https://www.python.org/ftp/python/2.7.13/python-2.7.13.msi" "https://www.python.org/ftp/python/2.7.13/python-2.7.13.msi.asc" \
		"https://raw.githubusercontent.com/thawsystems/docker-ida/master/ida-base/requirements-windows.txt" && \
	curl https://keybase.io/stevedower/pgp_keys.asc | gpg --import && \
	# gpg --recv-keys 7ED10B6531D7C8E1BC296021FC624643487034E5 && \
	gpg --verify python-2.7.13.msi.asc && \
	WINEDEBUG=fixme-all msiexec /i "python-2.7.13.msi" /passive /norestart ADDLOCAL=ALL ALLUSERS=1 InstallAllUsers=1 PrependPath=1 && \
	WINEDEBUG=fixme-all wine python -m pip install -U pip setuptools && \
	WINEDEBUG=fixme-all wine python -m pip install -r requirements-windows.txt && \
	rm /home/ida/.wine/dosdevices/z\: && ln -s /shared /home/ida/.wine/dosdevices/z\: && \
	rm -vrf python-2.7.13.msi python-2.7.13.msi.asc requirements-windows.txt 
