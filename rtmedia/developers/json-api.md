## JSON API

We recommend that you use these APIs first on a test site, before using them in production.

### API Syntax and usage

#### API Endpoint

[http://example.com/wp-admin/admin-ajax.php](http://example.com/wp-admin/admin-ajax.php)

We are using WordPress admin-ajax endpoint actions to implement rtMedia JSON API calls.

### Parameters

#### action => rtmedia_api

Action will be always equal to *rtmedia_api*. This will always be required.

#### method => method_name

This is another important parameter. It will identify the exact rtMedia method to be called on the server side.

#### Other Parameters

Depending on the method you want to call, you may need to pass one or more parameters.

## API Methods

### WordPress-Related Methods

This group of methods implements **register**, **login**, **logout**, **forgot-password**. Login methods also provide tokens to be used for subsequent calls for other methods.

**Response ** Format****


```error
status: TRUE or FALSE, on request success or failure
status_code: error/success code based on request method
message: success/error message
data:
success: array containing access_token or data on success
error: returns null
```

#### [POST] wp_register

**Request Format**

```error

action => rtmedia_api
method	=> wp_register
field_1 => user display name
signup_username => user login name
signup_email	=> user_email,
signup_password	=> user password
signup_password_confirm => user password repeat

```

**Response Sample for Successful Registration**

```error
{"status":"TRUE",
    "status_code":300007,
    "message":"new user created",
    "data":false
}
```


**Response Sample for Registration Failure (Fields Empty)**

```error
{"status":"FALSE",
    "status_code":300001,
    "message":"fields empty",
    "data":false
}
```
#### [POST] wp_login

**Request Format**

```error
action => rtmedia_api
    method	=> wp_login
    username => wordpress username
    password => wordpress password
```

**Response Sample for Successful Login**


```error
{"status":"TRUE",
    "status_code":200004,
    "message":"login success",
    "data":{
    "access_token":"1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1"
    }
  }
```

**Response Sample for Login Failure**

```error
{"status":"FALSE",
    "status_code":200003,
    "message":"incorrect password",
    "data":false
}
```

#### [POST] wp_logout

**Request Format**

```error
action => rtmedia_api
    method	=> wp_logout
    token	=> 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
```

**Response Sample for Successful Logout**

```error
{"status":"TRUE",
    "status_code":200005,
    "message":"logged out",
    "data":false
}
```

**Response Sample for Logout Failure (Invalid Token)**

```error
{"status":"FALSE",
    "status_code":600003,
    "message":"token invalid",
    "data":false
}
```

#### [POST] wp_forgot_password

**Request Format**

```error
action => rtmedia_api
    method	=> wp_forgot_password
    user_login => email
```

**Response Sample for Forgot Password Success**

```error
{"status":"TRUE",
    "status_code":500003,
    "message":"reset link sent",
    "data":false
}
```

**Response Sample for Forgot Password Failure (Invalid Email)**

```error
{"status":"FALSE",
    "status_code":500002,
    "message":"username/email not registered",
    "data":false
}
```

### BuddyPress-Related Methods

This group of methods implements **bp_get_activtities**, **bp_get_profile** and requires token to access data.

#### [GET] bp_get_activities

The method returns recent BuddyPress activities on the site. If `activity_user_id` is provided, it returns user specific activity.

**Request Format**

```
action => rtmedia_api
    method	=> bp_get_activities
    token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
    page => <strong>optional</strong>, page number to obtain feeds from next page, default 1
    per_page => <strong>optional</strong>, Activity Per Page , 10 by default
    activity_user_id => <strong>optional</strong>, to get user specific activity
```

**Response Sample for bp_get_activities Success**

```
"status":"TRUE",
"status_code":700001,
"message":"bp activities",
"data":{ "total_activity_count":10, "total":10, "current":1, "0": {"id":"17", "activity_type":"rtmedia_update", "activity_time":"2014-01-13 10:29:25", "activity_time_human":" 23 hours, 6 minutes ago", "activity_content":"<div class="rtmedia-activity-container"> <ul class="rtmedia-list large-block-grid-3"> <li class="rtmedia-list-item media-type-document"> <div class="rtmedia-item-thumbnail"> <a href="http://example.com/members/admin/media/57/"> <img src="http://example.com/wp-content/plugins/rtmedia-pro/app/assets/img/documents-icon.png" height="70" width="70" /></a> </div> <div class="rtmedia-item-title"> <a href="http://example.com/members/admin/media/57/">license.txt</a> </div> <div class="rtmedia-item-actions"></div> </li> </ul> </div>", "user":{"id":"1", "name":"admin", "avatar":"http://gravatar.com/avatar/b71b87b10ab35253f5cdd5d7e4492d03?d=http://example.com/wp-content/plugins/buddypress/bp-core/images/mystery-man.jpg&amp;s=80&amp;r=G" }, "media":[ {"id":"57", "title":"license.txt", "src":"http://example.com/wp-content/plugins/rtmedia-pro/app/assets/img/documents-icon.png", "likes":"0", "current_user":"FALSE" } ] } }
}
```

**Response Sample for bp_get_activities Failure (Invalid Token)**

```
{"status":"FALSE",
    "status_code":600003,
    "message":"token invalid",
    "data":false
 }
```

#### [GET] bp_get_profile

The function returns User avatar, Buddypress Profile Fields

**Request Format**

```
action => rtmedia_api
    method	=> bp_get_profile
    token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
    user_id	=> <strong>optional</strong>, fetches the profile of other user
```

**Response Sample for bp_get_profile Success**

```
{"status":"TRUE",
"status_code":400002,
"message":"profile fields",
"data":{"id":"1", "avatar":{ "src":"http://gravatar.com/avatar/b71b87b10ab35253f5cdd5d7e4492d03?d=http://example.com/wp-content/plugins/buddypress/bp-core/images/mystery-man.jpg&amp;s=250&amp;r=G", "width":250, "height":250 }, "fields":{ "Name":{"value":"adminn", "privacy":"public" } } }
}
```

### RTMedia-Related Methods

These methods implement comments, media likes, media add, rtmedia_gallery and requires token for data processing.

#### Media Upload

Details of the method to upload a new media using RTMedia API.

##### [POST] rtmedia_upload_media (under development)

API currently supports only uploading of images through a multipart form data.

**Request Format**
```
action => rtmedia_api
method	=> rtmedia_upload_media
token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
rtmedia_file => file input ( input type file )
title => <strong>optional, </strong>file name by default
context => profile(default)
image_type => jpeg/png
```

**Response Sample for rtmedia_uplaod_media success**

```
{"status":"TRUE",
"status_code":140004,
"message":"media updated",
"data":false
}
```

**Response Sample for rtmedia_uplaod_media failure**

```
{"status":"FALSE",
"status_code":140001,
"message":"no file",
"data":false
}
```

#### Comment Methods

Methods available for adding, removing comments for a media, list comments

##### [POST] add_rtmedia_comment

This method can be used to add a comment to media. The method requires `media_id`, `comment content` and `user token`.

**Request Format**

```
action => rtmedia_api
method	=> add_rtmedia_comment
token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
media_id => 47
content => example content
```

**Response Sample for add_rtmedia_comment Success**

```
{"status":"TRUE",
"status_code":800002,
"message":"comment posted",
"data":false
}
```

**Response Sample for add_rtmedia_comment Failure**

```
{"status":"FALSE",
"status_code":600006,
"message":"invalid media id",
"data":false
}
```

##### [GET] get_rtmedia_comment

Fetches comments for a user or media if you provide the media_id

**Request Format**

```
action => rtmedia_api
method	=> get_rtmedia_comment
token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
media_id => 47 ( <strong>optional, </strong>to fetch comments for a particular media )
```

**Response Sample for get_rtmedia_comment Success (For User)**

```
{"status":"TRUE",
"status_code":800004,
"message":"media comments",
"data":{"comments":[
 {"comment_ID":"3",
 "comment_content":"Facebok Stylern",
 "media_id":"23"
},
{"comment_ID":"4",
 "comment_content":"http://example.com/hello-world/trackback/",
  "media_id":"1"
 }
],
"user":{"user_id":"1",
    "name":"admin",
    "avatar":"http://gravatar.com/avatar/b71b87b10ab35253f5cdd5d7e4492d03?d=http://example.com/wp-content/plugins/buddypress/bp-core/images/mystery-man.jpg&amp;s=80&amp;r=G"
    }
 }
}
```

**Response Sample for get_rtmedia_comment Success (For Media)**

```
{"status":"TRUE",
 "status_code":800004,
 "message":"media comments",
 "data":{"comments":[ {"comment_ID":"7", "comment_content":"New Comment", "user_id":"2" }, {"comment_ID":"11", "comment_content":"example comment", "user_id":"1" } ], "user":{"2":{"name":"Test rtmedia", "avatar":"http://gravatar.com/avatar/ead51efe56285de1d46ca8a817a44f10?d=http://example.com/wp-content/plugins/buddypress/bp-core/images/mystery-man.jpg&amp;s=80&amp;r=G"},"1":{"name":"admin","avatar":"http://gravatar.com/avatar/b71b87b10ab35253f5cdd5d7e4492d03?d=http://example.com/wp-content/plugins/buddypress/bp-core/images/mystery-man.jpg&amp;s=80&amp;r=G" } } }
  }
```

##### [POST] remove_comment

**Request Format**

```
action => rtmedia_api
method	=> remove_comment
token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
media_id => 47 (media id obtained from buddypress activity or rtmedia_gallery)
comment_id => 11 (comment should be added by current logged in user)

```

**Response Sample for remove_comment Success**

```
{"status":"TRUE",
"status_code":800009,
"message":"comment deleted",
"data":false
}

```

**Response Sample for remove_comment Failure**

If comment id provided is associated with a different user, the method returns invalid id.

```
{"status":"FALSE",
"status_code":800007,
"message":"invalid comment/media id",
"data":false
}

```

#### Like Methods

Methods available for like and unlike, get likes for a media

##### [POST] like_media

**Toggle:  **method allows to like and unlike a media

**Request Format**

```
action => rtmedia_api
method	=> like_rtmedia
token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
media_id => 47
```

**Response Sample for like_media ( Like )**

```
    {"status":"TRUE",
    "status_code":900002,
    "message":"liked media",
    "data":{"like_count":2}
    }
```

**Response Sample for like_media ( Unlike )**

```
    {"status":"TRUE",
    "status_code":900001,
    "message":"unliked media",
    "data":{"like_count":1}
    }
```

[GET] get_likes_rtmedia

**Request Format**

```
 action => rtmedia_api
 method	=> get_likes_rtmedia
 token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
 media_id => 47
```

**Response Sample for get_likes_rtmedia**

```
  {"status":"TRUE",
    "status_code":900004,
    "message":"media likes",
    "data":{"user":{ "2":{"name":"Test rtmedia", "avatar":"http://gravatar.com/avatar/ead51efe56285de1d46ca8a817a44f10?d=http://example.com/wp-content/plugins/buddypress/bp-core/images/mystery-man.jpg&amp;s=80&amp;r=G" } }, "likes":[ {"activity_time":"4 days", "user_id":"2" } ] }
    }
```

#### GET Media Methods

Methods available for fetching a single media or all media

##### [GET] rtmedia_gallery

rtmedia_gallery method can be used to get different types of media with available parameters for the method.

**Request Format**

```
    action => rtmedia_api
    method	=> rtmedia_gallery
    token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
    media_type => photo, video, music, document (<strong>optional, </strong>by default fetches all media types, specific media types are fetched if specified as comma separated or array of values)
    media_author => current user(default), if super admin, media for all users is fetched(default), if a super user specifies id for other media author, media for that author is fetched
    global => true(default), <strong>optional, </strong>if set to false, global items are not included
    context => profile ( <strong>optional, </strong>not set by default )
    context_id => <strong>optional</strong>
    order_by => media_id (default, <strong>optional</strong>)
    per_page => 5 (<strong>optional, </strong>set to 10 by default)
    page => 2 (<strong>optional, </strong>displays media for first page by default)
```

**Response Sample for rtmedia_gallery (Success)**

```
 {"status":"TRUE",
 "status_code":160002,
 "message":"media list",
 "data":[ {"id":"60", "media_title":"Check", "album_id":"1", "media_type":"photo", "media_author":"1", "url":"http://example.com/members/admin/media/60/", "cover":false } ]
    }
```

For more details refer: [gallery-shortcode](../features/shortcodes/gallery.md)

##### [GET] rtmedia_get_media_details

**Request Format**

```
action => rtmedia_api
method	=> rtmedia_get_media_details
token => 1c1486ad94f3eb155ce52d3dfe0842292ca4f0a1
media_id => 47
```

**Response Sample for rtmedia_get_media_details (Success)**

 ```
 {"status":"TRUE",
 "status_code":150002,
 "message":"single media",
    "data":{"total_activity_count":1, "total":1, "current":1, "0":{"id":"4", "activity_type":"rtmedia_update", "activity_time":"2014-01-10 11:46:51", "activity_time_human":" 4 days, 1 hour ago", "activity_content":"<div class="rtmedia-activity-container"> <ul class="rtmedia-list large-block-grid-3"> <li class="rtmedia-list-item media-type-photo"> <a href="http://example.com/members/admin/media/47/"> <div class="rtmedia-item-thumbnail"> <img src="http://example.com/wp-content/uploads/rtMedia/users/1/2014/01/1-Fashion-106-320x240.jpg" /> </div> <div class="rtmedia-item-title">1 Fashion 106.jpg</div> </a> <div class="rtmedia-item-actions"></div> </li> </ul> </div>", "user":{"id":"1", "name":"admin", "avatar":"http://gravatar.com/avatar/b71b87b10ab35253f5cdd5d7e4492d03?d=http://example.com/wp-content/plugins/buddypress/bp-core/images/mystery-man.jpg&amp;s=80&amp;r=G"},"comments":{"comments":[{"comment_ID":"7","comment_content":"New Comment","user_id":"2"}],"user":{"2":{"name":"Test rtmedia","avatar":"http://gravatar.com/avatar/ead51efe56285de1d46ca8a817a44f10?d=http://example.com/wp-content/plugins/buddypress/bp-core/images/mystery-man.jpg&amp;s=80&amp;r=G" } } }, "media":[ {"id":"47", "title":"1 Fashion 106.jpg", "src":"http://example.com/wp-content/uploads/rtMedia/users/1/2014/01/1-Fashion-106-320x240.jpg", "likes":"1", "current_user":"FALSE"} ] } }
    }
```
