### Her
---

https://github.com/remiprev/her

```
gem "her"
# config/intializers/her.rb
Her::API.setup url: "https://api.ex.com" do |c|
  c.use Faraday::Request::UrlEncoded
  c.use Her::Middleware::DefaultParseJSON
  c.use Faraday::Adapter::NetHttp
end

class User
  include Her::Model
end

User.all
# => https://api.ex.com/users
User.find(1)
# => https://api.ex.com/users/1
@user = User.create(fullname: "Taka tky")
# POST https://api.ex.com/users fullname=Taka+tky
@user = User.new(fullname: "Taka tky")
@user.occupation = "actor"
@user.save
# POST https://api.ex.com/users fullname=Taka+tky&occupation=actor
@user.find(1)
@user.fullname = "Taka tky"
@user.save
# PUT https://api.ex.com/users/1 fullname=Taka+tky

class User
  include Her::Model
end
user = User.find(1)
user.fullname = "Taka tky"
user.save
User.save_existing(1, fullname: "Taka tky")
user = User.find(1)
user.destroy
User.destroy_existing(1)
User.all
User.where(moderator: 1).all
User.create(fullname: "Taka tky")
user = User.new(fullname: "Taka tky")
user.save!

# app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  before_filter :set_api_token
  
  protected
  def set_user_api_token
    RequestStore[my_api_token] = current_user.api_token
  end
end
# lib/my_token_authentication.rb
class MyTokenAuthentication < Faraday::Middleware
  def call(env)
    env[:request_headers]["X-API-Token"] = RequestStore.store[:my_api_token]
    @app.call(env)
  end
end
# config/initializers/her.rb
require "lib/my_token_authentication"
Her::API.setup url: "https://api.ex.com" do |c|
  c.use MyToeknAuthentication
  c.use Faraday::Request::UrlEncoded
  c.use Her::Middleware::DefaultParseJSON
  c.use Faraday::Adapter::NetHttp
end

# config/initializers/her.rb
Her::API.setup url: "https://api.ex.com" do |c|
  c.use Faraday::Request::BasicAuthentication, 'myusername', 'password'
  c.use Faraday::Request::UrlEncoded
  c.use Her::Middleware::DefaultParseJSON
  c.use Faraday::Adapter::NetHttp
end
```
```
gem "her"
gem "faraday_middleware"
gem "simple_oauth"

TWITTER_CREDENTIALS = {
  consumer_key: "",
  consumer_secret: "",
  token: "",
  token_secret: ""
}
Her::API.setup url: "https://api.twitter.com/1/" do |c|
  c.use FaradayMiddleware::OAuth, TWITTER_CREDENTIALS
  c.use Her::Middleware::DefaultParseJSON
  c.use Faraday::Adapter::NetHttp
end
class Tweet
  include Her::Model
end
@tweets = Tweet.get("/statuses/home_timeline.json")

{ "id" : 1, "name" " "Tky" }
[{ "id" : 1, "name" : "Tky" }]

class MyCustomeParser < Faraday::Response::Middleware
  def on_complete(env)
    json = MultiJson.load(env[body], symbolize_keys: true)
    env[:body] = {
      data: json[:result],
      errors: json[:errors],
      metadata: json[:metadata]
    }
  end
end
Her::API.setup url: "https://api.ex.com" do |c|
  c.use MyCustomParser
  c.use Faraday::Adapter::NetHttp
end

gem "her"
gem "faraday_middleware"
gem "memcached"
Her::API.setup url: "" do |c|
  c.use FaradayMiddleware::Caching, Memcached::Rails.new('127.0.0.1:11211')
  c.user Her::Middleware::DefaultParseJSON
  c.use Faraday::Adapter::NetHttp
end
class User
  include Her::Model
end
@user = User.find(1)
# GET "/users/1"
@user = User.find(1)


class User
  include Her::Model
  has_many :comments
  has_one :role
  belogns_to :organization
end
class Comment
  include Her::Model
end
class Role
  include Her::Model
end
class Organization
  include Her::Model
end

@user = User.find(1)
@user.comments
@user.role
@user.organization

@user = User.find(1)
@user.comments
@user.comments.where(approved: 1)
@user.role
@user.organization

@user = User.find(1)
@user.comments.build(body: "")
@user.comments.create(body: "", user_id: 1)

class Comment
  include Her::Model
  request_new_object_on_build true
end

class User
  include Her::Model
  collection_path "organizations/:organization_id/users"
end
class Organization
  include Her::Model
  has_many :users
end

Her::Errors::PathError: Missing :_organization_id parameter to build the request path. Path is `organizations/:organization_id/users`. Parameters are `{...}`

class User
  include Her::Model
  belongs_to :owns, class_name: "Organization"
end
class Organization
  include Her::Model
  has_many :owners, class_name: "User"
end

class User
  include Her::Model
  attributes :fullname, :email
  validates :fullname, presence: true
  validates :email, presence: true
end
@user = User.new(fullname: "Tky tky")
@user.valid?
@user.save
# POST "/users" `fullname=Tky_tky`

class User
  include Her::Model
  attributes :fullname, :email
end
class User
  include Her::Model
  before_save :set_internal_id
  after_find { |u| u.fillname.upcase! }
  def set_interval_id
    self.internal_id = 42
  end
end
@user = User.create(fullname: "Tky tky")
@user = User.find(1)
@user.fullname 

class User
  include Her::Model
  include_root_in_json true
end
class Article
  include Her::Model
  include_root_in_json :post
end
User.create(fullname: "tky tky")
Article.create(title: "tky tky")

class User
end
class Article
end
user = User.create(fullname: "")
user.fullname
article = Article.create(title: "")
article.title

class User
  include Her::Model
  parse_root_in_json true, format: :active_model_serializers
end
user = Users.find(1)
users = Users.all
```
```
{ "": {
  "": "",
  "": "",
  "": {
    "": "",
    "": "",
  }
}
}

class Contributor
end

Her::API.setup url: 'https://my_awesome_json_api_service' do |c|
end

class User
end
User.popular
User.unpopular
User.from_default(name: "")

class User
end
User.get(:popular)
User.get(:single_best)

class User
end
User.total

class User
end
User.get("/users/popular")

class User
end
@user = User.find(1)

class User
end
@user = User.find(1, _organization_id: 2)
@user = User.all(_organization_id: 2)
@user = User.new(fullname: "", organization_id: 2)
@user.save

class User
end
user = User.find()
user.destroy

module MyAPI
end
class User < MyAPI::Model
end
User.find(1)

class User
end
@admins = User.admins
@moderators = User.by_role()
@active_admins = User.active.admins

class User
end
@user = User.for_organization().find()
@user = User.for_organization().create()

# config/initializers/her.rb
MY_API = Her::API.new
MY_API.setup url: "" do |c|
end
OTHER_API = HER::API.new
OTHER_API.setup url: "" do ||
end

class User
end
class Category
end
User.all
Category.all

ssl_option = {}
Her::API.setup url: "", ssl:ssl_options do ||
end

# app/models/user.rb
class User
end
# app/models/post.rb
class Post
end

# spec/spec_helper.rb
RSpec.configure do |config|
end

# spec/models/user.rb
describe User do
end

# spec/models/user.rb
descirbe Post do
  descirbe :recent do
    before do
      stub_api_for(Post) do |stub|
        stub.get("/posts/recent") { |env| [200, {}, [{}, {}].to_json] }
      end
      subject { Post.recent }
      its(:length) { should == 2}
      its(:errors) { should be_empty }
    end
  end
  describe :archived do
    before do
      stub_api_for(Post) do |stub|
        stub.get("/posts/archived") { |env| [200, {}, [{ id: 1 }, { id: 2 }].to_json] }
      end
    end
    subject { Post.archived }
    its(:length) { should == 2}
    its(:errors) { should be_empty }
  end
end

```

