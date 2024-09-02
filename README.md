# Important Info

Files going to backend are Blobs

Files going to frontend are base64

# Controllers

## UserController:

### api/user/new POST

creates a new user if username and email non exist, returns session jwt

Input:
```
{
	"method": "POST",
	"url": "baseUrl/api/user/new",
	"headers": {
		"Content-Type": "application/json"
	},

	"body": {
		"username": string,
	    "password": string,
		"email": string,
		"birthDay": Date,
		"country": string
	}
}
```

Output:
```
{
	token: string
}
```

### api/user/logIn POST

user login, returns session jwt

Input
```
{
	"method": "POST",
	"url": "baseUrl/api/user/logIn",
	"headers": {
		"Content-Type": "application/json"
	},

	"body": {
            "username": string,
	    "password": string
	}
}
```

Output
```
{
    "token": string
}
```

### api/user/password-recovery POST

Input
```
{
	"method": "POST",
	"url": "baseUrl/api/user/password-recovery",
	"headers": {
		"Content-Type": "application/json"
	},

	"body": {
		"recoveryEmail": string
	}
}
```

Output
```
{
	"message": "email sended"
}
```

### api/user/password-change POST

```
{
	"method": "POST",
	"url": "baseUrl/api/user/password-change",
	"headers": {
		"Authorization": string (temporal resetToken in email link),
		"Content-Type": "application/json"
	},

	"body": {
		"username": string,
		"password": string
	}
}
```

### api/user/refreshToken GET

returns a new token giving the old one

Input
```
{
	"method": "GET",
	"url": "baseUrl/api/user/refresh-token",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	}
}
```

Output
```
{
    "token": string
}
```

### api/user/profile-pic-change PUT

Add or change user pfp

Input
```
{
	"method": "PUT",
	"url": "baseUrl/api/user/profile-pic-change",
	"headers": {
		"Authorization": string,
		"Content-Type": "multipart/form-data"
	},
	
	"body": formData
}
```

code for generating formData in profile-pic-change, use same key naming as example:
```
const formData = new FormData();
formData.append('profilePicture', profilePictureFile); 
```

Output
```
{
	"message": "new profile pic updated"
}
```
### api/user/profile-change PUT

Change or add their porfile's info

Input
```
{
	"method": "PUT",
	"url": "baseUrl/api/user/profile-change",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	},
	
	"body":{
		"description": string,
		"country": string
	}
}
```

Output
```
{
	"message": "profile update succesful"
}
```

### api/user/profile/{username}

Input
```
{
	"method": "GET",
	"url": "baseUrl/api/user/profile/{username}",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	}
}
```

Output
```
	"message": "get profile info succesful"
	"profile": {
		"avatar": string,
		"username": string,
		"description": string,
		"country": string,
		"rated": [
			{
			"comicReference": string, 
			"comicName": string, 
			"comicCover": string,
			"userRate": number,
			"averageRate": number 
			}
		]   
	} 
```
## ComicController:

### api/comic/all GET

returns filtered comics by url variables

Input
```
{
	"method": "GET",
	"url": "baseUrl/api/comic/all?title=...author=...&secureMode=true/false",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	}
}
```

url varibles
- **title**: string
- **author**: string
- **genre**: string
- **type**: string (manhwa, manga, comic...)
- **secureMode**: boolean (if true, excludes nsfw content, taken from user options)
- **items**: number (requested items per page)

urlExample: baseUrl/api/comic/all?author=someone&genre=action&items=20

Output
```
{
	"comics": [{
		"reference": string,
		"name": string,
		"cover": string
		"type": string,
		"totalChapters": number,
		"genres": [string]
	}],
	"pagination": {
		"totalItems": number,
		"totalPages": number,
		"actualPage": number
	}
}
```
### api/comic/trends GET

returns trend comics

Input
```
{
	"method": "GET",
	"url": "baseUrl/api/comic/trends?secureMode=true/false",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	}
}
```

Output
```
{
	"comics": [{
		"reference": string,
		"name": string,
		"cover": string,
		"type": string,
		"totalChapters": number,
		"genres": [string]
	}],
	"pagination": {
		"totalItems": number,
		"totalPages": number,
		"actualPage": number
	}
}
```
### api/comic/top GET

returns top comics

Input
```
{
	"method": "GET",
	"url": "baseUrl/api/comic/top?secureMode=true/false",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	}
}
```

Output
```
{
	"comics": [{
		"reference": string,
		"name": string,
		"cover": string,
		"type": string,
		"totalChapters": number,
		"genres": [string]
	}],
	"pagination": {
		"totalItems": number,
		"totalPages": number,
		"actualPage": number
	}
}
```
### api/comic/new POST

Add new comics, users and admins

```
{
	"method": "POST",
	"url": "baseUrl/api/comic/new",
	"headers": {
		"Authorization": string,
		"Content-Type": "multipart/form-data"
	},

	"body": formData
}
```

code structure for formData, use the same key naming as example:
```
const formData = new FormData();
formData.append('comicCover', comicCoverFile);
for (let i = 0; i < chapterFiles.length; i++) { 
	formData.append('chapters', chapterFiles[i]);
}
formData.append('data', {
	"name": string,
	"author": string,
	"releaseDate": Date,
	"genres": [string]
	"typeId": string,
	"Synopsis": string,
	"pegi": number,
	"nsfw": boolean,
	"firstChapterName": string
})
```

Output
```
{
	"message": "comic post succesful"
}
```
### api/comic/{reference} GET

get specific comic by id

Input
```
{
	"method": "GET",
	"url": "baseUrl/api/comic/{reference}",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	},
}
```

Output
```
{
	"comic": {
		"reference": string,
		"name": string,
		"author": string,
		"synopsis": string,
		"rate": number,
		"totalChapters": number,
		"timesVisitedThisWeek": number,
		"pegi": number,
	}
	"chapters": [
		{
		"name": string, 
		"index": number
		}
	]
}
```
### api/comic/{reference} DELETE

deletes specified comic

Input
```
{
	"method": "DELETE",
	"url": "baseUrl/api/comic/{reference}",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	},
}
```

Output
```
{
	"message": "delete succesful"
}
```
### api/comic/{reference} PUT

Comic's owner and admins can modify comic info

Input
```
{
	"method": "PUT",
	"url": "baseUrl/api/comic/{reference}",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	},

	"body": {
		"genreId": string,
		"typeId": string,
		"Synopsis": string,
		"pegi": number,
		"rate": number
	}
}
```

Output
```
{
	"message": "comic edition succesful"
}
```

### api/comic/rate/{reference} PUT

rate a comic, from 0 to 10, one decimal. example: 9.0, 1.8 etc etc

Input
```
{
	"method": "PUT",
	"url": "baseUrl/api/comic/rate/{reference}",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	},

	"body":{
		"rate": number
	}
}
```

Output
```
{
	"message": "rate succesful"
}
```
## ChapterController:

### api/chapter/get?comicId=?&chapter=? GET

returns the chapter info

Input
```
{
	"method": "PUT",
	"url": "baseUrl/api/chapter?comicId=string&chapter=number",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	}
}
```

Output
```
{
	"comic":{
		"name": string
	}
	"chapter":{
		"index": number
		"name": string
		"images": [{"image": string, "index": number}]
	}
	"comments":[
		{
			"username": string,
			"date": Date, 
			"text": string, 
			"likes": number, 
			"dislikes": number
		}
	]
}
```
### api/chapter/new POST

```
{
	"method": "POST",
	"url": "baseUrl/api/chapter/new",
	"headers": {
		"Authorization": string,
		"Content-Type": "multipart/form-data"
	},

	"body": formData
}
```

Code structure to build formData, use same key naming as example:
```
const formData = new FormData();
for (let i = 0; i < chapterFiles.length; i++) { 
	formData.append('chapters', chapterFiles[i]);
}
formData.append('data', {
	"chapterName": string
})
```
## CommentController
### api/comment/new POST

```
{
	"method": "POST",
	"url": "baseUrl/api/chapter/comment/new",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	},

	"body": {
		"chapterReference": string,
		"text": string,
	}
}
```

### api/comment/like PUT

Like a comment (if "like": true) or dislike it (if "like": false)

Input
```
{
	"method": "PUT",
	"url": "baseUrl/api/chapter/comment/like",
	"headers": {
		"Authorization": string,
		"Content-Type": "application/json"
	},

	"body": {
		"commentReference": number,
		"like": boolean
	}
}
```

Output
```
{
	"message": "succesful"
}
```
