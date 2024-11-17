# gitlab-ee-crack
可以运行容器后，在容器中生成证书，激活gitlab-ee

1. 创建容器
   ```bash
   docker run -d \
    --name='gitlab-ee' \
    gitlab/gitlab-ee:latest
   ```
2. 进入容器
   ```bash
   docker exec -it gitlab-ee bash
   ```

3. 生成证书
   ```bash
   # 先将文件gitlab_crack.py拷贝到容器中
   # 在容器中执行如下命令

   pip3 install pycryptodome==3.18.0
   
   # 此脚本会在当前目录生成：license_encryption_key.pub、GitLabEE.gitlab-license
   python3 gitlab_crack.py  


   mv ./.license_encryption_key.pub /opt/gitlab/embedded/service/gitlab-rails/.license_encryption_key.pub

   sed -i 's/restricted_attr(:plan).presence || STARTER_PLAN/restricted_attr(:plan).presence || ULTIMATE_PLAN/g' /opt/gitlab/embedded/service/gitlab-rails/ee/app/models/license.rb

   # 再重启容器

   ```

4. 进入管理后台上传许可证
   
   管理员->应用设置->通用->添加许可证

   将上一步生成的GitLabEE.gitlab-license上传即可

