# ====================================================================== #
# Android SDK Docker Image
# ====================================================================== #

# Base image
# ---------------------------------------------------------------------- #
#FROM oh-android-sdk
FROM android-sdk-arm
ARG USER=android
ARG USER_ID=1000

# Author
# ---------------------------------------------------------------------- #
LABEL maintainer "david.lee@outcomehealth.com"
RUN useradd -d /home/$USER -u $USER_ID $USER

RUN mkdir -p /home/$USER && \
    chown $USER_ID.$USER_ID /home/$USER 

WORKDIR /home/$USER
ADD dot.gradle .gradle
RUN chown -R $USER.$USER .gradle


USER $USER
# download and install Kotlin compiler
# https://github.com/JetBrains/kotlin/releases/latest

#### Install to sdk standard location 
RUN cp -a ${ANDROID_HOME} $HOME/sdk
ENV ANDROID_HOME $HOME/sdk
#RUN sdk/tools/bin/sdkmanager --update
RUN sdk/cmdline-tools/latest/bin/sdkmanager --update
RUN sdk/cmdline-tools/latest/bin/sdkmanager "build-tools;29.0.3" "platform-tools" "platforms;android-29" "platforms;android-14"


# setup adb server
EXPOSE 5037
CMD "/bin/bash"
