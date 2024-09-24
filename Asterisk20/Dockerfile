# ========================================
# Asterisk With Siperb Registration
# See details from Admin Control Panel below
# Please see https://www.siperb.com/ fore more.
# ========================================

FROM debian:bookworm

# Asterisk Details
ENV ASTERISK_VERSION=releases/20

# Siperb Details
ENV SIPERB_USR=_________________________
ENV SIPERB_PSW=_________________________
ENV SIPERB_SBC=eu-west-1-sbc-1.siperb.com
ENV SIPERB_IP=52.50.165.204

# ========================================

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -y git iputils-ping traceroute
WORKDIR /usr/local/src
# Download src
RUN git clone --branch ${ASTERISK_VERSION} --single-branch --depth 1 https://github.com/asterisk/asterisk.git
# Install asterisk
WORKDIR /usr/local/src/asterisk
RUN contrib/scripts/install_prereq install
RUN ./configure
RUN make menuselect.makeopts
RUN menuselect/menuselect \
 --disable BUILD_NATIVE \
 --disable-all \
 --enable chan_bridge_media \
 --enable chan_rtp \
 --enable chan_pjsip \
 --enable bridge_native_rtp \
 --enable bridge_simple \
 --enable codec_gsm \
 --enable codec_a_mu \
 --enable codec_alaw \
 --enable codec_ulaw \
 --enable codec_opus \
 --enable codec_resample \
 --enable format_gsm \
 --enable format_wav \
 --enable format_wav_gsm \
 --enable format_pcm \
 --enable format_ogg_vorbis \
 --enable format_h264 \
 --enable format_h263 \
 --enable func_base64 \
 --enable func_callerid \
 --enable func_channel \
 --enable func_curl \
 --enable func_cut \
 --enable func_db \
 --enable func_logic \
 --enable func_math \
 --enable func_sprintf \
 --enable func_strings \
 --enable app_confbridge \
 --enable app_db \
 --enable app_dial \
 --enable app_echo \
 --enable app_exec \
 --enable app_mixmonitor \
 --enable app_originate \
 --enable app_playback \
 --enable app_playtones \
 --enable app_queue \
 --enable app_sendtext \
 --enable app_stack \
 --enable app_transfer \
 --enable app_system \
 --enable app_verbose \
 --enable app_voicemail \
 --enable app_externalivr \
 --enable pbx_config \
 --enable pbx_realtime \
 --enable res_musiconhold \
 --enable res_agi \
 --enable res_ari \
 --enable res_ari_applications \
 --enable res_ari_asterisk \
 --enable res_ari_bridges \
 --enable res_ari_channels \
 --enable res_ari_device_states \
 --enable res_ari_endpoints \
 --enable res_ari_events \
 --enable res_ari_mailboxes \
 --enable res_ari_model \
 --enable res_ari_playbacks \
 --enable res_ari_recordings \
 --enable res_ari_sounds \
 --enable res_clioriginate \
 --enable res_config_curl \
 --enable res_config_odbc \
 --enable res_curl \
 --enable res_format_attr_h263 \
 --enable res_format_attr_h264 \
 --enable res_format_attr_opus \
 --enable res_format_attr_vp8 \
 --enable res_http_post \
 --enable res_http_websocket \
 --enable res_odbc \
 --enable res_odbc_transaction \
 --enable res_parking \
 --enable res_pjproject \
 --enable res_pjsip \
 --enable res_pjsip_acl \
 --enable res_pjsip_authenticator_digest \
 --enable res_pjsip_caller_id \
 --enable res_pjsip_dialog_info_body_generator \
 --enable res_pjsip_diversion \
 --enable res_pjsip_dlg_options \
 --enable res_pjsip_dtmf_info \
 --enable res_pjsip_empty_info \
 --enable res_pjsip_endpoint_identifier_anonymous \
 --enable res_pjsip_endpoint_identifier_ip \
 --enable res_pjsip_endpoint_identifier_user \
 --enable res_pjsip_exten_state \
 --enable res_pjsip_header_funcs \
 --enable res_pjsip_logger \
 --enable res_pjsip_messaging \
 --enable res_pjsip_mwi \
 --enable res_pjsip_mwi_body_generator \
 --enable res_pjsip_nat \
 --enable res_pjsip_notify \
 --enable res_pjsip_one_touch_record_info \
 --enable res_pjsip_outbound_authenticator_digest \
 --enable res_pjsip_outbound_publish \
 --enable res_pjsip_outbound_registration \
 --enable res_pjsip_path \
 --enable res_pjsip_pidf_body_generator \
 --enable res_pjsip_pidf_digium_body_supplement \
 --enable res_pjsip_pidf_eyebeam_body_supplement \
 --enable res_pjsip_publish_asterisk \
 --enable res_pjsip_pubsub \
 --enable res_pjsip_refer \
 --enable res_pjsip_registrar \
 --enable res_pjsip_rfc3326 \
 --enable res_pjsip_sdp_rtp \
 --enable res_pjsip_send_to_voicemail \
 --enable res_pjsip_session \
 --enable res_pjsip_sips_contact \
 --enable res_pjsip_t38 \
 --enable res_pjsip_transport_websocket \
 --enable res_pjsip_xpidf_body_generator \
 --enable res_realtime \
 --enable res_rtp_asterisk \
 --enable res_sorcery_astdb \
 --enable res_sorcery_config \
 --enable res_sorcery_memory \
 --enable res_sorcery_memory_cache \
 --enable res_sorcery_realtime \
 --enable res_srtp \
 --enable OPTIONAL_API \
 --enable MOH-OPSOUND-WAV \
 --enable CORE-SOUNDS-EN-WAV \
 menuselect.makeopts
RUN	make all
RUN make install
RUN make clean

# Postinstall
RUN chmod -R 750 /var/spool/asterisk
RUN rm -rf /var/lib/apt/lists/*
RUN rm -rf /usr/local/src/asterisk

# Make own samples
WORKDIR /etc/asterisk/
COPY Docker/config/* /etc/asterisk/

# Replaces
RUN sed -i "s/{{SIPERB_USERNAME}}/${SIPERB_USR}/g" pjsip.conf
RUN sed -i "s/{{SIPERB_SECRET}}/${SIPERB_PSW}/g" pjsip.conf
RUN sed -i "s/{{SIPERB_SBC}}/${SIPERB_SBC}/g" pjsip.conf
RUN sed -i "s/{{SIPERB_IP}}/${SIPERB_IP}/g" pjsip.conf

EXPOSE 5060/udp

HEALTHCHECK --interval=60s --timeout=10s --retries=3 CMD /usr/sbin/asterisk -rx "core show sysinfo"

ENTRYPOINT ["/usr/sbin/asterisk","-f"]

CMD ["-v"]

# Build this docker:
# docker build --tag asterisk20:latest .
# or
# docker build --no-cache --tag asterisk20:latest .

# Run
# docker run -t -i -p 5060:5060/udp -p 8080:8080/tcp -p 8089:8089/tcp asterisk20:latest
# -p 8000-10000:8000-10000/udp