FROM ruby:2.5
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs sqlite
RUN gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
RUN gem install rails
# Set working directory
RUN mkdir /myapp
WORKDIR /myapp
#COPY Gemfile /myapp/Gemfile
#COPY Gemfile.lock /myapp/Gemfile.lock
# Adding project files
COPY . /myapp
RUN bundle install
EXPOSE 3000
