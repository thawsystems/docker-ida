FROM thawsystems/innounp as extracter

ARG IDA_INSTALLATION_FILE=idasetup.exe
ARG IDA_PASSWORD

COPY ${IDA_INSTALLATION_FILE} /home/innounp/

RUN innounp -x -d/home/innounp/extracted -p${IDA_PASSWORD} -b /home/innounp/${IDA_INSTALLATION_FILE}


FROM thawsystems/ida-base

COPY --from=extracter /home/innounp/extracted/\{app\} /home/ida/extracted

USER root
RUN mv /home/ida/extracted /home/ida/.wine/drive_c/ida && chown -R ida:ida /home/ida/.wine/drive_c/ida

USER ida
WORKDIR /shared

ENTRYPOINT ["/usr/local/bin/ida_service.sh"]
