FROM centos:7

ARG chef_version="16.8.14"
ARG chef_version_revision="-1"
ARG chef_url="https://packages.chef.io/files/stable/chef/$chef_version/el/7/chef-${chef_version}${chef_version_revision}.el7.x86_64.rpm"
# Known issue with ruby_version=27
ARG ruby_version=26
ARG LD_LIBRARY_PATH=/opt/rh/rh-ruby${ruby_version}/root/usr/lib64/
ENV LD_LIBRARY_PATH=/opt/rh/rh-ruby${ruby_version}/root/usr/lib64/
ARG PATH="/opt/rh/rh-ruby${ruby_version}/root/usr/bin:$PATH"
ENV PATH="/opt/rh/rh-ruby${ruby_version}/root/usr/bin:$PATH"

WORKDIR /mnt

ADD cookbook-elasticsearch/Gemfile /tmp/Gemfile

RUN yum install -y centos-release-scl \
  && yum upgrade -y \
  && yum install -y git rh-ruby${ruby_version}-rubygem-bundler rh-ruby${ruby_version}-ruby-devel \
  && yum install -y 
  && yum install -y gcc make rpm-build zlib-devel libxml2-devel curl-devel gcc-c++ openssl-devel \
  && bundle install --gemfile=/tmp/Gemfile \
  && rm -f /tmp/Gemfile \
  && bundle exec gem install github_changelog_generator \
  && curl -o chef-client.rpm "$chef_url" \
  && yum install -y chef-client.rpm \
  && rm -f chef-client.rpm \
  && yum clean all
