# ====================================================================== #
# Android SDK Docker Image
# ====================================================================== #

# Base image
# ---------------------------------------------------------------------- #
FROM ubuntu:18.04

ARG GRADLE_VERSION=5.2.1
ARG KOTLIN_VERSION=1.3.50
ARG ANDROID_SDK_VERSION=4333796
ENV ANDROID_HOME /opt/android-sdk

# Author
# ---------------------------------------------------------------------- #
LABEL maintainer "david.lee@outcomehealth.com"
WORKDIR /opt

# support multiarch: i386 architecture
# install Java
# install essential tools
# install Qt
RUN dpkg --add-architecture i386 && \
    apt-get update -y && \
    apt-get install -y --no-install-recommends libncurses5:i386 libc6:i386 libstdc++6:i386 lib32gcc1 lib32ncurses5 lib32z1 zlib1g:i386 && \
    apt-get install -y --no-install-recommends git wget unzip 
RUN mkdir -p /usr/lib/jvm 
ADD jdk-8u221-linux-x64.tar.gz  /usr/lib/jvm
RUN update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_221/bin/java 1 && \
   update-alternatives --config java


# download and install Gradle
# https://services.gradle.org/distributions/
# download and install Kotlin compiler
# https://github.com/JetBrains/kotlin/releases/latest
ADD license_accepter.sh .
ADD https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip  /opt
RUN unzip gradle*.zip && \
    ls -d */ | sed 's/\/*$//g' | xargs -I{} mv {} gradle  &&  \
    rm -f gradle*.zip
ADD https://github.com/JetBrains/kotlin/releases/download/v${KOTLIN_VERSION}/kotlin-compiler-${KOTLIN_VERSION}.zip /opt
RUN    unzip *kotlin*.zip && rm *kotlin*.zip

# download and install Android SDK
# https://developer.android.com/studio/#downloads
RUN mkdir -p ${ANDROID_HOME} 
ADD https://dl.google.com/android/repository/sdk-tools-linux-${ANDROID_SDK_VERSION}.zip  ${ANDROID_HOME}
RUN cd ${ANDROID_HOME} ; \
    unzip *tools*linux*.zip && \
    rm *tools*linux*.zip

# set the environment variables
ENV JAVA_HOME /usr/lib/jvm/jdk1.8.0_221
ENV GRADLE_HOME /opt/gradle
ENV KOTLIN_HOME /opt/kotlinc
ENV PATH ${PATH}:${GRADLE_HOME}/bin:${KOTLIN_HOME}/bin:${ANDROID_HOME}/emulator:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/tools/bin
ENV _JAVA_OPTIONS -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap
# WORKAROUND: for issue https://issuetracker.google.com/issues/37137213
ENV LD_LIBRARY_PATH ${ANDROID_HOME}/emulator/lib64:${ANDROID_HOME}/emulator/lib64/qt/lib

# accept the license agreements of the SDK components
RUN /opt/license_accepter.sh $ANDROID_HOME

#### Install to sdk standard location 
##RUN cd ${ANDROID_HOME} ; \
 ##   sdkmanager --update && \
 ##   sdkmanager "build-tools;29.0.2" "platform-tools" "platforms;android-29" "platforms;android-14"


# setup adb server
EXPOSE 5037
CMD "/bin/bash"
