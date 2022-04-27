# [Choice] Ruby version (use -bullseye variants on local arm64/Apple Silicon): 3, 3.1, 3.0, 2, 2.7, 2.6, 3-bullseye, 3.1-bullseye, 3.0-bullseye, 2-bullseye, 2.7-bullseye, 2.6-bullseye, 3-buster, 3.1-buster, 3.0-buster, 2-buster, 2.7-buster, 2.6-buster
ARG VARIANT=3.1-bullseye
FROM mcr.microsoft.com/vscode/devcontainers/ruby:0-${VARIANT}

# Install Rails
RUN gem install rails:6.0.4 yard bundler:2.2.17 solargraph 

# [Choice] Node.js version: lts/*, 16, 14, 12, 10
ARG NODE_VERSION="lts/*"
RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"

# Clean up
RUN apt-get autoremove -y \
    && curl -sL https://deb.nodesource.com/setup_16.x | bash - \
    && apt-get update \
    && apt-get -y install git nodejs sudo gosu \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

RUN npm install -g yarn heroku

RUN echo "vscode ALL=NOPASSWD: ALL" >> /etc/sudoers

ENV SHELL /bin/bash

WORKDIR /workspace
COPY Gemfile /workspace
COPY Gemfile.lock /workspace

RUN yard config --gem-install-yri 
RUN bundle config set --local without 'production' 
RUN bundle update 
RUN bundle install 
RUN yard gems
#RUN yard config --gem-install-yri \
  #&& bundle config set --local without 'production' \
  #&& bundle update \
  #&& bundle install \
  #&& yard gems

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment this line to install additional gems.
# RUN gem install <your-gem-names-here>

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1
