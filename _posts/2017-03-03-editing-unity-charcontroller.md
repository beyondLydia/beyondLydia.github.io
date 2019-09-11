---
layout: post
title: unity的character controller方法修改
---
[搬运2014年旧笔记]

物体方向控制方法

    function Update(){
          if(Input.GetKey(KeyCode.W){
               transform.Translate(Vector3.forward*Time.deltaTime*2);
          }else if(Input.GetKey(KeyCode.S){
               transform.Translate(Vector3.forward*Time.deltaTime*-2);
          }else if(Input.GetKey(KeyCode.A){
               transform.Translate(Vector3.up*Time.deltaTime*-20);
          }else if(Input.GetKey(KeyCode.A){
               transform.Translate(Vector3.up*Time.deltaTime*20);
          }
     }

修改unity自带character controller代码，实现鼠标右键按下后再旋转视角的功能。（可查阅MouseLook.cs）

    if (Input.GetMouseButton (1)) {
        if (axes == RotationAxes.MouseXAndY) {//鼠标右键按下后再旋转视角
            float rotationX = transform.localEulerAngles.y + Input.GetAxis (“Mouse X”) * sensitivityX;
            rotationY += Input.GetAxis (“Mouse Y”) * sensitivityY;
            rotationY = Mathf.Clamp (rotationY, minimumY, maximumY);
            transform.localEulerAngles = new Vector3 (-rotationY, rotationX, 0);
            } else if (axes == RotationAxes.MouseX) {
                transform.Rotate (0, Input.GetAxis (“Mouse X”) * sensitivityX, 0);
            } else {
                rotationY += Input.GetAxis (“Mouse Y”) * sensitivityY;
                rotationY = Mathf.Clamp (rotationY, minimumY, maximumY);
                transform.localEulerAngles = new Vector3 (-rotationY, transform.localEulerAngles.y, 0);
                }

