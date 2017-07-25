FROM thawsystems/innounp as extracter

ARG IDA_INSTALLATION_FILE=idasetup.exe
ARG IDA_PASSWORD

COPY ${IDA_INSTALLATION_FILE} /home/innounp/

RUN innounp -x -d/home/innounp/extracted -p${IDA_PASSWORD} -b /home/innounp/${IDA_INSTALLATION_FILE}


FROM thawsystems/ida-base

COPY --from=extracter /home/innounp/extracted/\{app\} /home/ida/extracted

USER root
RUN mv /home/ida/extracted /home/ida/.wine/drive_c/ida && chown -R ida:ida /home/ida/.wine/drive_c/ida && \
	mkdir /shared && chown ida:ida /shared

USER ida

# Create a special file in order to prevent IDA to ask for license acceptance before executing IDA
# RUN touch /ida/license.displayed

WORKDIR /shared

# ENV PATH /ida:$PATH
# ENV TERM xterm
# ENV PYTHONPATH /usr/local/lib/python2.7/dist-packages:/usr/local/lib/python2.7/site-packages:/usr/lib/python2.7/:$PYTHONPATH

# ENTRYPOINT ["/usr/local/bin/ida_service.sh"]
