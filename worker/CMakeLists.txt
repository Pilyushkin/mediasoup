set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if (MSVC)
    list(APPEND TARGET_COMPILE_OPTIONS /W2)
else ()
    list(APPEND TARGET_COMPILE_OPTIONS -Wall)
endif ()

message(STATUS "Endianness: ${CMAKE_CXX_BYTE_ORDER}")
if (${CMAKE_CXX_BYTE_ORDER} STREQUAL LITTLE_ENDIAN)
    list(APPEND TARGET_COMPILE_DEFINITIONS MS_LITTLE_ENDIAN)
elseif()
    list(APPEND TARGET_COMPILE_DEFINITIONS MS_BIG_ENDIAN)
endif()

if (MSVC)
    list(APPEND TARGET_COMPILE_DEFINITIONS
            # Don't define min and max macros (windows.h)
            NOMINMAX
            # Don't bloat namespace with incompatible winsock versions.
            WIN32_LEAN_AND_MEAN
            # Don't warn about usage of insecure C functions.
            _CRT_SECURE_NO_WARNINGS
            _SCL_SECURE_NO_WARNINGS
            # Introduced in VS 2017 15.8, allow overaligned types in aligned_storage.
            _ENABLE_EXTENDED_ALIGNED_STORAGE
            )
endif ()

option(MS_LOG_TRACE [[When enabled, logs the current method/function if current log level is "debug"]] OFF)
option(MS_LOG_FILE_LINE [[When enabled, all the logging macros print more verbose information, including current file and line]] OFF)
option(MS_RTC_LOGGER_RTP [[When enabled, prints a line with information for each RTP packet]] OFF)

if (MS_LOG_TRACE)
    list(APPEND TARGET_COMPILE_DEFINITIONS MS_LOG_TRACE)
endif ()

if (MS_LOG_FILE_LINE)
    list(APPEND TARGET_COMPILE_DEFINITIONS} MS_LOG_FILE_LINE)
endif ()

if (MS_RTC_LOGGER_RTP)
    list(APPEND TARGET_COMPILE_DEFINITIONS MS_RTC_LOGGER_RTP)
endif ()


list(APPEND COMMON_SOURCES
        src/lib.cpp
        src/DepLibSRTP.cpp
        src/DepLibUV.cpp
        src/DepLibWebRTC.cpp
        src/DepOpenSSL.cpp
        src/DepUsrSCTP.cpp
        src/Logger.cpp
        src/MediaSoupErrors.cpp
        src/Settings.cpp
        src/Worker.cpp
        src/ChannelMessageRegistrator.cpp
        src/Utils/Crypto.cpp
        src/Utils/File.cpp
        src/Utils/IP.cpp
        src/Utils/String.cpp
        src/handles/SignalsHandler.cpp
        src/handles/TcpConnectionHandler.cpp
        src/handles/TcpServerHandler.cpp
        src/handles/Timer.cpp
        src/handles/UdpSocketHandler.cpp
        src/handles/UnixStreamSocket.cpp
        src/Channel/ChannelNotifier.cpp
        src/Channel/ChannelRequest.cpp
        src/Channel/ChannelSocket.cpp
        src/PayloadChannel/PayloadChannelNotification.cpp
        src/PayloadChannel/PayloadChannelNotifier.cpp
        src/PayloadChannel/PayloadChannelRequest.cpp
        src/PayloadChannel/PayloadChannelSocket.cpp
        src/RTC/ActiveSpeakerObserver.cpp
        src/RTC/AudioLevelObserver.cpp
        src/RTC/Consumer.cpp
        src/RTC/DataConsumer.cpp
        src/RTC/DataProducer.cpp
        src/RTC/DirectTransport.cpp
        src/RTC/DtlsTransport.cpp
        src/RTC/IceCandidate.cpp
        src/RTC/IceServer.cpp
        src/RTC/KeyFrameRequestManager.cpp
        src/RTC/NackGenerator.cpp
        src/RTC/PipeConsumer.cpp
        src/RTC/PipeTransport.cpp
        src/RTC/PlainTransport.cpp
        src/RTC/PortManager.cpp
        src/RTC/Producer.cpp
        src/RTC/RateCalculator.cpp
        src/RTC/Router.cpp
        src/RTC/RtcLogger.cpp
        src/RTC/RtpListener.cpp
        src/RTC/RtpObserver.cpp
        src/RTC/RtpPacket.cpp
        src/RTC/RtpProbationGenerator.cpp
        src/RTC/RtpRetransmissionBuffer.cpp
        src/RTC/RtpStream.cpp
        src/RTC/RtpStreamRecv.cpp
        src/RTC/RtpStreamSend.cpp
        src/RTC/RtxStream.cpp
        src/RTC/SctpAssociation.cpp
        src/RTC/SctpListener.cpp
        src/RTC/SenderBandwidthEstimator.cpp
        src/RTC/SeqManager.cpp
        src/RTC/Shared.cpp
        src/RTC/SimpleConsumer.cpp
        src/RTC/SimulcastConsumer.cpp
        src/RTC/SrtpSession.cpp
        src/RTC/StunPacket.cpp
        src/RTC/SvcConsumer.cpp
        src/RTC/TcpConnection.cpp
        src/RTC/TcpServer.cpp
        src/RTC/Transport.cpp
        src/RTC/TransportCongestionControlClient.cpp
        src/RTC/TransportCongestionControlServer.cpp
        src/RTC/TransportTuple.cpp
        src/RTC/TrendCalculator.cpp
        src/RTC/UdpSocket.cpp
        src/RTC/WebRtcServer.cpp
        src/RTC/WebRtcTransport.cpp
        src/RTC/Codecs/H264.cpp
        src/RTC/Codecs/H264_SVC.cpp
        src/RTC/Codecs/VP8.cpp
        src/RTC/Codecs/VP9.cpp
        src/RTC/Codecs/Opus.cpp
        src/RTC/RtpDictionaries/Media.cpp
        src/RTC/RtpDictionaries/Parameters.cpp
        src/RTC/RtpDictionaries/RtcpFeedback.cpp
        src/RTC/RtpDictionaries/RtcpParameters.cpp
        src/RTC/RtpDictionaries/RtpCodecMimeType.cpp
        src/RTC/RtpDictionaries/RtpCodecParameters.cpp
        src/RTC/RtpDictionaries/RtpEncodingParameters.cpp
        src/RTC/RtpDictionaries/RtpHeaderExtensionParameters.cpp
        src/RTC/RtpDictionaries/RtpHeaderExtensionUri.cpp
        src/RTC/RtpDictionaries/RtpParameters.cpp
        src/RTC/RtpDictionaries/RtpRtxParameters.cpp
        src/RTC/SctpDictionaries/SctpStreamParameters.cpp
        src/RTC/RTCP/Packet.cpp
        src/RTC/RTCP/CompoundPacket.cpp
        src/RTC/RTCP/SenderReport.cpp
        src/RTC/RTCP/ReceiverReport.cpp
        src/RTC/RTCP/Sdes.cpp
        src/RTC/RTCP/Bye.cpp
        src/RTC/RTCP/Feedback.cpp
        src/RTC/RTCP/FeedbackPs.cpp
        src/RTC/RTCP/FeedbackRtp.cpp
        src/RTC/RTCP/FeedbackRtpNack.cpp
        src/RTC/RTCP/FeedbackRtpTmmb.cpp
        src/RTC/RTCP/FeedbackRtpSrReq.cpp
        src/RTC/RTCP/FeedbackRtpTllei.cpp
        src/RTC/RTCP/FeedbackRtpEcn.cpp
        src/RTC/RTCP/FeedbackRtpTransport.cpp
        src/RTC/RTCP/FeedbackPsPli.cpp
        src/RTC/RTCP/FeedbackPsSli.cpp
        src/RTC/RTCP/FeedbackPsRpsi.cpp
        src/RTC/RTCP/FeedbackPsFir.cpp
        src/RTC/RTCP/FeedbackPsTst.cpp
        src/RTC/RTCP/FeedbackPsVbcm.cpp
        src/RTC/RTCP/FeedbackPsLei.cpp
        src/RTC/RTCP/FeedbackPsAfb.cpp
        src/RTC/RTCP/FeedbackPsRemb.cpp
        src/RTC/RTCP/XR.cpp
        src/RTC/RTCP/XrDelaySinceLastRr.cpp
        src/RTC/RTCP/XrReceiverReferenceTime.cpp
        )

list(APPEND PUBLIC_HEADER
        include/common.hpp
        include/lib.hpp
        )

include(FetchContent)

set(OPENSSL_USE_STATIC_LIBS ON)
#todo Local path openssl
#find_package(OpenSSL 3 REQUIRED) #todo remove fetchContent for openssl
FetchContent_Declare(
        openssl-cmake
        URL https://github.com/jimmy-park/openssl-cmake/archive/main.tar.gz
        FIND_PACKAGE_ARGS NAMES OpenSSL 3
)

if(MEDIASOUP_INSTALL) #todo ?
    set(JSON_Install ON)
endif()
FetchContent_Declare(
        nlohmann_json
        URL https://github.com/nlohmann/json/releases/download/v3.10.5/json.tar.xz
        DOWNLOAD_NAME nlohmann_json-3.10.5.zip
        FIND_PACKAGE_ARGS NAMES nlohmann_json 3.10
)

FetchContent_Declare(
        libuv
        URL
        https://dist.libuv.org/dist/v1.44.2/libuv-v1.44.2.tar.gz
        https://github.com/mesonbuild/wrapdb/releases/download/libuv_1.44.2-2/libuv-v1.44.2.tar.gz
        FIND_PACKAGE_ARGS NAMES uv_a 1.44
)

set(TEST_APPS OFF)
FetchContent_Declare(
        libsrtp2
        URL https://github.com/cisco/libsrtp/archive/refs/tags/v2.5.0.tar.gz
        DOWNLOAD_NAME libsrtp-2.5.0.tar.gz
        URL_HASH SHA256=8a43ef8e9ae2b665292591af62aa1a4ae41e468b6d98d8258f91478735da4e09
        FIND_PACKAGE_ARGS NAMES libSRTP
)

option(sctp_build_programs OFF)
FetchContent_Declare(
        usrsctp
        URL https://github.com/sctplab/usrsctp/archive/4e06feb01cadcd127d119486b98a4bd3d64aa1e7.zip
        URL_HASH SHA256=15f7844c4c4ca93228ae0fe844182c72edd1d809b461cb97b1bb687a804dd4fc
        FIND_PACKAGE_ARGS NAMES usrsctp
)

if(MEDIASOUP_INSTALL)
    set(ABSL_ENABLE_INSTALL ON)
endif()
set(ABSL_PROPAGATE_CXX_STD ON)
FetchContent_Declare(
        abseil_cpp
        URL https://github.com/abseil/abseil-cpp/archive/20220623.0.tar.gz
        DOWNLOAD_NAME abseil-cpp-20220623.0.tar.gz
        URL_HASH SHA256=d19cb16610d9310658a815ebcd87a9e2966aafbd57964341c0d1a3a3778c03b6
        FIND_PACKAGE_ARGS NAMES absl 20220623
)


FetchContent_MakeAvailable(
        openssl-cmake
        nlohmann_json
        libuv
        libsrtp2
        usrsctp
        abseil_cpp
)


list(APPEND MEDIASOUP_INCLUDE_DIRS
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        )

add_subdirectory(deps/libwebrtc)

list(
        APPEND MEDIASOUP_DEPENDENCIES
        OpenSSL::SSL
        OpenSSL::Crypto
        uv_a # libuv (static)
        libSRTP::srtp2
        nlohmann_json::nlohmann_json
        usrsctp
        absl::base
        absl::strings
        absl::flat_hash_map
        libwebrtc::libwebrtc
)

if (MSVC)
    FetchContent_Declare(
            wingetopt
            URL https://github.com/alex85k/wingetopt/archive/v1.00.zip
            DOWNLOAD_NAME wingetopt-1.00.zip
            URL_HASH SHA256=4454ca03a59702a4ca4d1488ca8fa6168b0c8d77dc739a6fe2825c3dd8609d87
    )
    list(APPEND MEDIASOUP_DEPENDENCIES wingetopt)
endif ()

set(LIBRARY_NAMESPACE mediasoup)

set(LIBRARY_TARGET libmediasoup-worker)
add_library(${LIBRARY_TARGET} STATIC ${COMMON_SOURCES} #[[$<TARGET_OBJECTS:libwebrtc::libwebrtc>]])
add_library(${LIBRARY_NAMESPACE}::${LIBRARY_TARGET} ALIAS ${LIBRARY_TARGET})

set_target_properties(${LIBRARY_TARGET} PROPERTIES PUBLIC_HEADER "${PUBLIC_HEADER}")

target_compile_options(${LIBRARY_TARGET} PUBLIC ${TARGET_COMPILE_OPTIONS})
target_compile_definitions(${LIBRARY_TARGET} PUBLIC ${TARGET_COMPILE_DEFINITIONS})

target_include_directories(${LIBRARY_TARGET} PUBLIC
        "$<BUILD_INTERFACE:${MEDIASOUP_INCLUDE_DIRS}>"
        "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/${LIBRARY_NAMESPACE}>"
        )
target_link_libraries(${LIBRARY_TARGET} PUBLIC ${MEDIASOUP_DEPENDENCIES})


set(BINARY_TARGET mediasoup-worker)
add_executable(${BINARY_TARGET} src/main.cpp)
target_compile_definitions(${BINARY_TARGET} PRIVATE MS_EXECUTABLE)
target_link_libraries(${BINARY_TARGET} PRIVATE ${LIBRARY_TARGET})

################### tests ###################

if (MEDIASOUP_BUILD_TESTS)

    FetchContent_Declare(
            catch2
            URL https://github.com/catchorg/Catch2/archive/v2.13.7.zip
            DOWNLOAD_NAME Catch2-2.13.7.zip
            URL_HASH SHA256=3f3ccd90ad3a8fbb1beeb15e6db440ccdcbebe378dfd125d07a1f9a587a927e9
            FIND_PACKAGE_ARGS NAMES Catch2 2.13
    )
    FetchContent_MakeAvailable(catch2)

    list(APPEND TEST_SOURCES
            test/src/tests.cpp
            test/src/PayloadChannel/TestPayloadChannelNotification.cpp
            test/src/PayloadChannel/TestPayloadChannelRequest.cpp
            test/src/RTC/TestKeyFrameRequestManager.cpp
            test/src/RTC/TestNackGenerator.cpp
            test/src/RTC/TestRateCalculator.cpp
            test/src/RTC/TestRtpPacket.cpp
            test/src/RTC/TestRtpPacketH264Svc.cpp
            test/src/RTC/TestRtpRetransmissionBuffer.cpp
            test/src/RTC/TestRtpStreamSend.cpp
            test/src/RTC/TestRtpStreamRecv.cpp
            test/src/RTC/TestSeqManager.cpp
            test/src/RTC/TestTrendCalculator.cpp
            test/src/RTC/TestRtpEncodingParameters.cpp
            test/src/RTC/Codecs/TestVP8.cpp
            test/src/RTC/Codecs/TestVP9.cpp
            test/src/RTC/Codecs/TestH264.cpp
            test/src/RTC/Codecs/TestH264_SVC.cpp
            test/src/RTC/RTCP/TestFeedbackPsAfb.cpp
            test/src/RTC/RTCP/TestFeedbackPsFir.cpp
            test/src/RTC/RTCP/TestFeedbackPsLei.cpp
            test/src/RTC/RTCP/TestFeedbackPsPli.cpp
            test/src/RTC/RTCP/TestFeedbackPsRemb.cpp
            test/src/RTC/RTCP/TestFeedbackPsRpsi.cpp
            test/src/RTC/RTCP/TestFeedbackPsSli.cpp
            test/src/RTC/RTCP/TestFeedbackPsTst.cpp
            test/src/RTC/RTCP/TestFeedbackPsVbcm.cpp
            test/src/RTC/RTCP/TestFeedbackRtpEcn.cpp
            test/src/RTC/RTCP/TestFeedbackRtpNack.cpp
            test/src/RTC/RTCP/TestFeedbackRtpSrReq.cpp
            test/src/RTC/RTCP/TestFeedbackRtpTllei.cpp
            test/src/RTC/RTCP/TestFeedbackRtpTmmb.cpp
            test/src/RTC/RTCP/TestFeedbackRtpTransport.cpp
            test/src/RTC/RTCP/TestBye.cpp
            test/src/RTC/RTCP/TestReceiverReport.cpp
            test/src/RTC/RTCP/TestSdes.cpp
            test/src/RTC/RTCP/TestSenderReport.cpp
            test/src/RTC/RTCP/TestPacket.cpp
            test/src/RTC/RTCP/TestXr.cpp
            test/src/Utils/TestBits.cpp
            test/src/Utils/TestByte.cpp
            test/src/Utils/TestIP.cpp
            test/src/Utils/TestJson.cpp
            test/src/Utils/TestString.cpp
            test/src/Utils/TestTime.cpp
            )


    set(BINARY_TEST_TARGET mediasoup-worker-test)
    add_executable(${BINARY_TEST_TARGET} ${TEST_SOURCES})
    target_compile_options(${BINARY_TEST_TARGET} PRIVATE ${TARGET_COMPILE_OPTIONS})
    target_compile_definitions(${BINARY_TEST_TARGET} PRIVATE ${TARGET_COMPILE_DEFINITIONS} MS_LOG_STD MS_TEST)
    target_include_directories(${BINARY_TEST_TARGET} PRIVATE ${MEDIASOUP_INCLUDE_DIRS} test/include)
    target_link_libraries(${BINARY_TEST_TARGET} PRIVATE ${LIBRARY_TARGET} Catch2::Catch2)

    add_test(NAME ${BINARY_TEST_TARGET} COMMAND ${BINARY_TEST_TARGET} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

    set(BINARY_TEST_TARGET_ASAN mediasoup-worker-test-asan)
    add_executable(${BINARY_TEST_TARGET_ASAN} ${TEST_SOURCES})
    target_compile_options(${BINARY_TEST_TARGET_ASAN} PRIVATE ${TARGET_COMPILE_OPTIONS} -fsanitize=address)
    target_link_options(${BINARY_TEST_TARGET_ASAN} PRIVATE ${TARGET_COMPILE_OPTIONS} -fsanitize=address)
    target_compile_definitions(${BINARY_TEST_TARGET_ASAN} PRIVATE ${TARGET_COMPILE_DEFINITIONS} MS_LOG_STD MS_TEST)
    target_include_directories(${BINARY_TEST_TARGET_ASAN} PRIVATE ${MEDIASOUP_INCLUDE_DIRS} test/include)
    target_link_libraries(${BINARY_TEST_TARGET_ASAN} PRIVATE ${LIBRARY_TARGET} Catch2::Catch2)

    add_test(NAME ${BINARY_TEST_TARGET_ASAN} COMMAND ${BINARY_TEST_TARGET_ASAN} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

    if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
        list(APPEND FUZZER_SOURCES
                fuzzer/src/fuzzer.cpp
                fuzzer/src/FuzzerUtils.cpp
                fuzzer/src/RTC/FuzzerStunPacket.cpp
                fuzzer/src/RTC/FuzzerRtpPacket.cpp
                fuzzer/src/RTC/FuzzerRtpRetransmissionBuffer.cpp
                fuzzer/src/RTC/FuzzerRtpStreamSend.cpp
                fuzzer/src/RTC/FuzzerSeqManager.cpp
                fuzzer/src/RTC/FuzzerTrendCalculator.cpp
                fuzzer/src/RTC/RTCP/FuzzerBye.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPs.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPsAfb.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPsFir.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPsLei.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPsPli.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPsRemb.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPsRpsi.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPsSli.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPsTst.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackPsVbcm.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackRtp.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpEcn.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpNack.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpSrReq.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpTllei.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpTmmb.cpp
                fuzzer/src/RTC/RTCP/FuzzerFeedbackRtpTransport.cpp
                fuzzer/src/RTC/RTCP/FuzzerPacket.cpp
                fuzzer/src/RTC/RTCP/FuzzerReceiverReport.cpp
                fuzzer/src/RTC/RTCP/FuzzerSdes.cpp
                fuzzer/src/RTC/RTCP/FuzzerSenderReport.cpp
                fuzzer/src/RTC/RTCP/FuzzerXr.cpp
                )

        set(FUZZER_TARGET mediasoup-worker-fuzzer)
        add_executable(${FUZZER_TARGET} ${FUZZER_SOURCES})
        target_compile_options(${FUZZER_TARGET} PRIVATE ${TARGET_COMPILE_OPTIONS} -g -O0 -fsanitize=address,fuzzer)
        target_link_options(${FUZZER_TARGET} PRIVATE ${TARGET_COMPILE_OPTIONS} -fsanitize=address,fuzzer)
        target_compile_definitions(${FUZZER_TARGET} PRIVATE ${TARGET_COMPILE_DEFINITIONS} MS_LOG_STD)
        target_include_directories(${FUZZER_TARGET} PRIVATE ${MEDIASOUP_INCLUDE_DIRS} fuzzer/include)
        target_link_libraries(${FUZZER_TARGET} PRIVATE ${LIBRARY_TARGET} Catch2::Catch2)

        add_test(NAME ${FUZZER_TARGET} COMMAND ${FUZZER_TARGET} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

    endif ()
endif ()

if (MEDIASOUP_INSTALL AND NOT CMAKE_SKIP_INSTALL_RULES #[[AND OFF]])
    # Не получается сделать установку, так как у подпроектов нет install(EXPORT)
    #CMake Error: install(EXPORT "MediasoupTargets" ...) includes target "libmediasoup-worker" which requires target "usrsctp" that is not in any export set.
    #CMake Error: install(EXPORT "MediasoupTargets" ...) includes target "libmediasoup-worker" which requires target "libwebrtc" that is not in any export set.

    set(MEDIASOUP_COMPONENT ${LIBRARY_TARGET})
    set(MEDIASOUP_DEV_COMPONENT ${MEDIASOUP_COMPONENT}-dev)

    include(GNUInstallDirs)

    set(LIBRARY_TARGET_EXPORT MediasoupTargets)
    install(TARGETS ${LIBRARY_TARGET}
            EXPORT ${LIBRARY_TARGET_EXPORT}
            RUNTIME COMPONENT ${MEDIASOUP_COMPONENT}
            LIBRARY COMPONENT ${MEDIASOUP_COMPONENT}
            ARCHIVE COMPONENT ${MEDIASOUP_DEV_COMPONENT}
            PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR/${LIBRARY_NAMESPACE}}
            )

    set(EXPORTS_PATH "${CMAKE_INSTALL_LIBDIR}/${LIBRARY_NAMESPACE}/cmake")

    install(EXPORT ${LIBRARY_TARGET_EXPORT}
            DESTINATION ${EXPORTS_PATH}
            NAMESPACE ${LIBRARY_NAMESPACE}::
            )

    include(CMakePackageConfigHelpers)
    set(LIBRARY_TARGET_EXPORT_PATH ${EXPORTS_PATH}/${LIBRARY_TARGET_EXPORT}.cmake)
    configure_package_config_file(
            ${CMAKE_CURRENT_SOURCE_DIR}/MediasoupConfig.cmake.in
            "${CMAKE_CURRENT_BINARY_DIR}/MediasoupConfig.cmake"
            INSTALL_DESTINATION ${EXPORTS_PATH}
            PATH_VARS
            LIBRARY_TARGET_EXPORT_PATH
            MEDIASOUP_COMPONENT
            MEDIASOUP_DEV_COMPONENT
    )

    install(
            FILES "${CMAKE_CURRENT_BINARY_DIR}/MediasoupConfig.cmake"
            DESTINATION ${EXPORTS_PATH}
    )

    write_basic_package_version_file(
            "${CMAKE_CURRENT_BINARY_DIR}/MediasoupConfigVersion.cmake"
            COMPATIBILITY ExactVersion
    )

    install(
            FILES "${CMAKE_CURRENT_BINARY_DIR}/MediasoupConfigVersion.cmake"
            DESTINATION ${EXPORTS_PATH}
            )

    # todo  Install.md with components list

    #todo package option?
endif ()