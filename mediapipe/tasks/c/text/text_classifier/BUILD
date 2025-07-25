# Copyright 2023 The MediaPipe Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load("@rules_cc//cc:cc_binary.bzl", "cc_binary")
load("@rules_cc//cc:cc_library.bzl", "cc_library")
load("@rules_cc//cc:cc_test.bzl", "cc_test")

licenses(["notice"])

cc_library(
    name = "text_classifier_lib",
    srcs = ["text_classifier.cc"],
    hdrs = ["text_classifier.h"],
    visibility = ["//visibility:public"],
    deps = [
        "//mediapipe/tasks/c/components/containers:classification_result",
        "//mediapipe/tasks/c/components/containers:classification_result_converter",
        "//mediapipe/tasks/c/components/processors:classifier_options",
        "//mediapipe/tasks/c/components/processors:classifier_options_converter",
        "//mediapipe/tasks/c/core:base_options",
        "//mediapipe/tasks/c/core:base_options_converter",
        "//mediapipe/tasks/cc/text/text_classifier",
        "@com_google_absl//absl/log:absl_log",
        "@com_google_absl//absl/status",
    ],
    alwayslink = 1,
)

# bazel build -c opt --linkopt -s --strip always --define MEDIAPIPE_DISABLE_GPU=1 \
# //mediapipe/tasks/c/text/text_classifier:libtext_classifier.so
cc_binary(
    name = "libtext_classifier.so",
    linkopts = [
        "-Wl,-soname=libtext_classifier.so",
        "-fvisibility=hidden",
    ],
    linkshared = True,
    tags = [
        "manual",
        "nobuilder",
        "notap",
    ],
    deps = [":text_classifier_lib"],
)

# bazel build --config darwin_arm64 -c opt --strip always --define MEDIAPIPE_DISABLE_GPU=1 \
# //mediapipe/tasks/c/text/text_classifier:libtext_classifier.dylib
cc_binary(
    name = "libtext_classifier.dylib",
    linkopts = [
        "-Wl,-install_name,libtext_classifier.dylib",
        "-fvisibility=hidden",
    ],
    linkshared = True,
    tags = [
        "manual",
        "nobuilder",
        "notap",
    ],
    deps = [":text_classifier_lib"],
)

cc_test(
    name = "text_classifier_test",
    srcs = ["text_classifier_test.cc"],
    data = ["//mediapipe/tasks/testdata/text:bert_text_classifier_models"],
    linkstatic = 1,
    deps = [
        ":text_classifier_lib",
        "//mediapipe/framework/deps:file_path",
        "//mediapipe/framework/port:gtest",
        "//mediapipe/tasks/c/components/containers:category",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)
