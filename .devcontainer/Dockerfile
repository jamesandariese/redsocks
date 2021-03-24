# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.163.1/containers/ubuntu/.devcontainer/base.Dockerfile

# [Choice] Ubuntu version: bionic, focal
ARG VARIANT="focal"
FROM mcr.microsoft.com/vscode/devcontainers/go:${VARIANT} AS downloader


RUN mkdir /opt/android-sdk
RUN curl -o /opt/android-sdk/clitools.zip https://dl.google.com/android/repository/commandlinetools-linux-6858069_latest.zip
RUN cd /opt/android-sdk && unzip clitools.zip && rm clitools.zip && mkdir tools && mv cmdline-tools/* tools/ && mv tools cmdline-tools/
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends default-jdk-headless
RUN yes | /opt/android-sdk/cmdline-tools/tools/bin/sdkmanager "platform-tools"
RUN yes | /opt/android-sdk/cmdline-tools/tools/bin/sdkmanager "platforms;android-28"
RUN yes | /opt/android-sdk/cmdline-tools/tools/bin/sdkmanager "ndk;22.0.7026061"

ENV ANDROID_SDK_ROOT=/opt/android-sdk

#RUN /opt/android-sdk/tools/bin/sdkmanager

FROM mcr.microsoft.com/vscode/devcontainers/go:${VARIANT} AS target

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends default-jdk-headless

RUN mkdir -p /opt && rm -rf /opt/android-sdk
COPY --from=downloader /opt/android-sdk/ /opt/android-sdk/
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV NDK=$ANDROID_SDK_ROOT/ndk/22.0.7026061/
ENV TOOLCHAIN=$NDK/toolchains/llvm/prebuilt/linux-x86_64
ENV TARGET=aarch64-linux-android
ENV AR=$TOOLCHAIN/bin/llvm-ar
ENV CC=$TOOLCHAIN/bin/$TARGET$API-clang
ENV AS=$CC
ENV CXX=$TOOLCHAIN/bin/$TARGET$API-clang++
ENV LD=$TOOLCHAIN/bin/ld
ENV RANLIB=$TOOLCHAIN/bin/llvm-ranlib
ENV STRIP=$TOOLCHAIN/bin/llvm-strip

RUN chown -R vscode:vscode /opt/android-sdk

# download gradle
#RUN ./gradlew --help > /dev/null
