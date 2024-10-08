#-------------------------------------------------------------------------------
# Dockerfile
# ============LICENSE_START=======================================================
#  Copyright (C) 2021 Nordix Foundation.
# ================================================================================
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
#
# SPDX-License-Identifier: Apache-2.0
# ============LICENSE_END=========================================================
#-------------------------------------------------------------------------------
FROM armdocker.rnd.ericsson.se/proj-sdd/pf-istanbul-new/common-base-images/eric-oss-pf-base-image:9.0.1

LABEL maintainer="Policy Team"

ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk
ENV POLICY_ETC /opt/app/policy/etc
ENV POLICY_PROFILE /opt/app/policy/etc/profile.d
ENV POLICY_BIN /opt/app/policy/bin

RUN zypper update \
    && zypper in -l -y shadow mariadb-client net-tools sudo less \
    && mkdir -p $POLICY_PROFILE $POLICY_BIN
# Create policy user and group
RUN groupadd policy && useradd -m policy -g policy \
    && chown -R policy:policy $POLICY_ETC $POLICY_BIN

COPY --chown=policy:policy ./env.sh $POLICY_PROFILE/
COPY --chown=policy:policy ./db-migrator $POLICY_BIN/
COPY --chown=policy:policy ./prepare_upgrade.sh $POLICY_BIN/
COPY --chown=policy:policy ./prepare_downgrade.sh $POLICY_BIN/
COPY --chown=policy:policy ./config/policyadmin/sql /home/policy/sql

WORKDIR $POLICY_BIN
USER policy:policy


