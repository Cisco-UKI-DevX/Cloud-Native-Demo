# Copyright 2020 Google LLC
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

FROM golang:1.15 as builder
WORKDIR /src

# restore dependencies
COPY appdynamics /usr/local/go/src/appdynamics
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN go build -o /go/bin/frontend .

FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y wget \
    && rm -rf /var/lib/apt/lists/*

COPY appdynamics/lib /lib
WORKDIR /frontend
COPY --from=builder /go/bin/frontend /frontend/server
COPY ./templates ./templates
COPY ./static ./static
EXPOSE 8080
ENTRYPOINT ["/frontend/server"]
