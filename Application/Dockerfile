FROM ubuntu:latest

ENV HOME=/root
ENV RUBY_VERSION=1.9.3-p551
ENV BUNDLE_VERSION=1.0.22

RUN apt-get update
RUN apt-get install -y --force-yes build-essential curl git
RUN apt-get install -y --force-yes zlib1g-dev libssl-dev libreadline-dev libyaml-dev libxml2-dev libxslt-dev libmysqlclient-dev
RUN apt-get clean

RUN git clone https://github.com/rbenv/rbenv.git $HOME/.rbenv
RUN git clone https://github.com/rbenv/ruby-build.git $HOME/.rbenv/plugins/ruby-build
RUN $HOME/.rbenv/plugins/ruby-build/install.sh
ENV PATH=$HOME/.rbenv/bin:$PATH
RUN echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh
RUN echo 'eval "$(rbenv init -)"' >> $HOME/.bashrc
RUN echo 'gem: --no-rdoc --no-ri' >> $HOME/.gemrc

RUN rbenv install $RUBY_VERSION
RUN bash -l -c 'rbenv global $RUBY_VERSION && gem install bundler -v $BUNDLE_VERSION'

WORKDIR $HOME
RUN mkdir webapp
WORKDIR $HOME/webapp

COPY Gemfile.lock .
COPY Gemfile .
COPY vendor vendor

RUN rbenv exec bundle install

COPY . .

EXPOSE 3000

CMD ["rbenv", "exec", "bundle", "exec", "rails", "s" ]
