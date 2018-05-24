---
layout: post
title: "Angular2 file upload"
tags: angular2 form
---

요즘 한창 Django 와 Angular2를 사용해서 웹사이트를 만들고 있다. 헌데 angular2에서 여러개의 file upload하는 방법이 정확하게 나와 있지 않아서 여기에 정리해 본다.



```html
<form  [formGroup]="myform" (ngSubmit)="submit()">
<label>Name</label>
<input name="name" type="text" formControlName="name">
<label>Image</label>
<input name="image" type="file" change="onChangeImage($event)">
```

우선 form의 형식은 위처럼 name과 image 두개.

```js
export class ProjectformComponent {
  public myform: FormGroup;
  private image: File;


  onChangeImage(event) {
    const files = event.target.files || event.srcElement.files;
    this.image = files[0];
  }

  submit(): void {
    // validate fields locally
    if(!this.image) {
      console.log('image is required.');
      return;
    }

    let data = new FormData();
    for( var p in this.myform.value) {
      data.append(p, this.myform.value[p]);
    }

    if(this.image) {
      data.append('image', this.image);
    }

    let header = new Headers({'enctype': 'multipart/form-data'});
    this.http.post('/api/url', data, header).subscribe(
        resp => console.log('response: ', resp),
        error => console.log(error)
      );
  }
}
```

위처럼 FormData를 만들어서 이미지와 일반 필드들의 값을 append한후 post의 data로 날리면 multipart로 서버로 전송된다. 나의 경우는 django rest framework을 사용해서 서버를 구성했다.






