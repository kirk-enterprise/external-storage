# Copyright 2017 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM centos:7

ENV CEPH_RELEASE "luminous"
ENV CEPH_VERSION "12.2.5"
RUN rpm --import 'https://download.ceph.com/keys/release.asc' && \
  rpm -Uvh https://download.ceph.com/rpm-$CEPH_RELEASE/el7/noarch/ceph-release-1-1.el7.noarch.rpm && \
  yum install -y epel-release && \
  yum install -y ceph-common-$CEPH_VERSION python-cephfs-$CEPH_VERSION && \
  yum install -y patch && \
  yum clean all

# patch https://github.com/ceph/ceph/pull/21808 for luminos 12.2.5
# TODO upgrade python-cephfs to new version which includes this patch when available
ADD patches /tmp/patches
RUN cd /usr/lib/python2.7/site-packages && \
  patch -p3 < /tmp/patches/21808.diff

COPY cephfs-provisioner /usr/local/bin/cephfs-provisioner
COPY cephfs_provisioner/cephfs_provisioner.py /usr/local/bin/cephfs_provisioner
RUN chmod -v o+x /usr/local/bin/cephfs_provisioner

