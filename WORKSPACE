# Copyright (C) 2018 The Flogger Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

http_archive(
    name = "google_bazel_common",
    strip_prefix = "bazel-common-e7580d1db7466e6c8403f7826b7558ea5e99bbfd",
    urls = ["https://github.com/google/bazel-common/archive/e7580d1db7466e6c8403f7826b7558ea5e99bbfd.zip"],
)

load("@google_bazel_common//:workspace_defs.bzl", "google_common_workspace_rules")

google_common_workspace_rules()
