list(APPEND LIBWEBRTC_SOURCES
        libwebrtc/system_wrappers/source/field_trial.cc
        libwebrtc/rtc_base/rate_statistics.cc
        libwebrtc/rtc_base/experiments/field_trial_parser.cc
        libwebrtc/rtc_base/experiments/alr_experiment.cc
        libwebrtc/rtc_base/experiments/field_trial_units.cc
        libwebrtc/rtc_base/experiments/rate_control_settings.cc
        libwebrtc/rtc_base/network/sent_packet.cc
        libwebrtc/call/rtp_transport_controller_send.cc
        libwebrtc/api/transport/bitrate_settings.cc
        libwebrtc/api/transport/field_trial_based_config.cc
        libwebrtc/api/transport/network_types.cc
        libwebrtc/api/transport/goog_cc_factory.cc
        libwebrtc/api/units/timestamp.cc
        libwebrtc/api/units/time_delta.cc
        libwebrtc/api/units/data_rate.cc
        libwebrtc/api/units/data_size.cc
        libwebrtc/api/units/frequency.cc
        libwebrtc/api/network_state_predictor.cc
        libwebrtc/modules/pacing/interval_budget.cc
        libwebrtc/modules/pacing/bitrate_prober.cc
        libwebrtc/modules/pacing/paced_sender.cc
        libwebrtc/modules/remote_bitrate_estimator/overuse_detector.cc
        libwebrtc/modules/remote_bitrate_estimator/overuse_estimator.cc
        libwebrtc/modules/remote_bitrate_estimator/aimd_rate_control.cc
        libwebrtc/modules/remote_bitrate_estimator/inter_arrival.cc
        libwebrtc/modules/remote_bitrate_estimator/bwe_defines.cc
        libwebrtc/modules/remote_bitrate_estimator/remote_bitrate_estimator_abs_send_time.cc
        libwebrtc/modules/rtp_rtcp/include/rtp_rtcp_defines.cc
        libwebrtc/modules/bitrate_controller/send_side_bandwidth_estimation.cc
        libwebrtc/modules/bitrate_controller/loss_based_bandwidth_estimation.cc
        libwebrtc/modules/congestion_controller/goog_cc/goog_cc_network_control.cc
        libwebrtc/modules/congestion_controller/goog_cc/probe_bitrate_estimator.cc
        libwebrtc/modules/congestion_controller/goog_cc/congestion_window_pushback_controller.cc
        libwebrtc/modules/congestion_controller/goog_cc/link_capacity_estimator.cc
        libwebrtc/modules/congestion_controller/goog_cc/alr_detector.cc
        libwebrtc/modules/congestion_controller/goog_cc/probe_controller.cc
        libwebrtc/modules/congestion_controller/goog_cc/median_slope_estimator.cc
        libwebrtc/modules/congestion_controller/goog_cc/bitrate_estimator.cc
        libwebrtc/modules/congestion_controller/goog_cc/trendline_estimator.cc
        libwebrtc/modules/congestion_controller/goog_cc/delay_based_bwe.cc
        libwebrtc/modules/congestion_controller/goog_cc/acknowledged_bitrate_estimator.cc
        libwebrtc/modules/congestion_controller/rtp/send_time_history.cc
        libwebrtc/modules/congestion_controller/rtp/transport_feedback_adapter.cc
        libwebrtc/modules/congestion_controller/rtp/control_handler.cc
        )

set(TARGET_NAME libwebrtc)
add_library(${TARGET_NAME} OBJECT ${LIBWEBRTC_SOURCES})
add_library(${TARGET_NAME}::${TARGET_NAME} ALIAS ${TARGET_NAME})

target_compile_options(${TARGET_NAME} PRIVATE ${TARGET_COMPILE_OPTIONS})
target_compile_definitions(${TARGET_NAME} PRIVATE ${TARGET_COMPILE_DEFINITIONS})

target_include_directories(${TARGET_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} libwebrtc PRIVATE ${MEDIASOUP_INCLUDE_DIRS})
target_link_libraries(${TARGET_NAME} ${MEDIASOUP_DEPENDENCIES})