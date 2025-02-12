# hexo-eternal-heart
README
项目概述
本项目旨在使用 Hexo 和 Java 实现一个完整的前后端项目，包括一个基于 Hexo 主题 Butterfly 的静态网站和一个用于管理博客内容的后台管理系统。此外，项目还支持视频播放、音频播放、工具展示和算法展示等功能，并提供方便的网站迁移方案以及将博客内容同步到 GitHub 的功能。
项目结构
Copy
project-root/
├── frontend/               # 前端项目目录
│   ├── hexo/               # Hexo 静态网站源码
│   │   ├── source/         # 博客内容源文件
│   │   ├── themes/         # Hexo 主题目录
│   │   │   └── butterfly/  # Butterfly 主题
│   │   └── ...
│   └── ...
├── backend/                # 后端项目目录
│   ├── src/                # Java 源代码
│   │   ├── main/           # 主代码目录
│   │   │   ├── java/       # Java 类文件
│   │   │   └── resources/  # 配置文件
│   │   └── test/           # 测试代码目录
│   └── ...
├── resources/              # 资源文件目录
│   ├── videos/             # 视频资源
│   ├── audios/             # 音频资源
│   ├── tools/              # 工具资源
│   └── algorithms/         # 算法资源
├── README.md               # 项目说明文档
└── ...
功能需求分析
1. Hexo 静态网站
   1.1 主题配置
   使用 Hexo 的 Butterfly 主题，确保主题的美观性和功能完整性。
   1.2 功能扩展
   视频播放
   视频资源存储于阿里云 OSS。
   在页面按分类展示视频信息列表，点击可跳转到详情页观看。
   音频播放
   音频资源存储于阿里云 OSS。
   提供全局可拖动的音乐播放器，可在博客中创建音乐播放器接管播放。
   工具展示
   展示个人制作的工具列表，点击可跳转到工具页面。
   算法展示
   展示不同算法的描述，提供实时运行的算法可视化展示，点击可跳转到算法页面。
2. 后台管理系统
   2.1 文章管理
   支持后台方便地编写文章，设置标签、分类和友链。
   2.2 资源管理
   支持后台方便地管理音乐、视频、工具、算法以及其他资源。
3. 网站迁移方案
   提供方便的网站迁移方案，确保网站数据的完整性和一致性。
4. 内容同步
   支持将博客的文档内容同步到 GitHub，便于前端项目的更新和维护。
   实现步骤
1. 初始化项目
   安装 Node.js 和 npm。
   安装 Hexo：
   bashCopy
   npm install -g hexo-cli
   初始化 Hexo 项目：
   bashCopy
   hexo init hexo
   cd hexo
   npm install
   安装 Butterfly 主题：
   bashCopy
   git clone https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
   配置 Hexo 和 Butterfly 主题：
   编辑 _config.yml 文件，配置 Hexo 和主题相关设置。
2. 功能实现
   2.1 视频播放
   在 _config.yml 中配置阿里云 OSS 的访问信息。
   创建视频分类页面模板，展示视频信息列表。
   在视频详情页中嵌入视频播放器，使用阿里云 OSS 的视频资源。
   2.2 音频播放
   在 _config.yml 中配置阿里云 OSS 的访问信息。
   创建全局可拖动的音乐播放器，使用阿里云 OSS 的音频资源。
   在博客页面中嵌入音乐播放器，支持接管播放。
   2.3 工具展示
   创建工具列表页面，展示工具的名称和描述。
   为每个工具创建单独的页面，提供工具的详细信息和使用方法。
   2.4 算法展示
   创建算法列表页面，展示算法的名称和简要描述。
   为每个算法创建单独的页面，提供算法的详细描述和可视化展示。
3. 后台管理系统
   使用 Java 框架（如 Spring Boot）搭建后台管理系统。
   实现文章管理功能，包括文章的增删改查。
   实现资源管理功能，包括音乐、视频、工具、算法等资源的增删改查。
   提供友好的后台管理界面，方便用户操作。
4. 网站迁移方案
   使用数据库备份工具定期备份网站数据。
   提供数据迁移脚本，确保网站数据的完整性和一致性。
5. 内容同步
   使用 Git 钩子或脚本，将博客内容同步到 GitHub。
   配置 GitHub Actions 或其他 CI/CD 工具，实现自动部署。
   关键代码示例
1. Hexo 配置文件 _config.yml
   yamlCopy
# Hexo 配置
title: Your Blog Title
subtitle: Your Blog Subtitle
description: Your Blog Description
author: Your Name
language: en
timezone: Asia/Shanghai

# Butterfly 主题配置
theme: butterfly
butterfly:
# 自定义配置
video:
enabled: true
oss:
endpoint: oss-cn-hangzhou.aliyuncs.com
bucket: your-bucket-name
accessKeyId: your-access-key-id
accessKeySecret: your-access-key-secret
audio:
enabled: true
oss:
endpoint: oss-cn-hangzhou.aliyuncs.com
bucket: your-bucket-name
accessKeyId: your-access-key-id
accessKeySecret: your-access-key-secret
2. 视频播放页面模板
   HTMLCopy
---
layout: video
title: Video Title
---

<div class="video-list">
  {% for video in site.data.videos %}
    <div class="video-item">
      <a href="{{ video.url }}">{{ video.title }}</a>
    </div>
  {% endfor %}
</div>
3. 后台管理系统文章管理接口
javaCopy
@RestController
@RequestMapping("/api/articles")
public class ArticleController {
    @Autowired
    private ArticleService articleService;

    @PostMapping
    public ResponseEntity<Article> createArticle(@RequestBody Article article) {
        return ResponseEntity.ok(articleService.save(article));
    }

    @GetMapping("/{id}")
    public ResponseEntity<Article> getArticle(@PathVariable Long id) {
        return ResponseEntity.ok(articleService.findById(id));
    }

    @PutMapping("/{id}")
    public ResponseEntity<Article> updateArticle(@PathVariable Long id, @RequestBody Article article) {
        return ResponseEntity.ok(articleService.update(id, article));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteArticle(@PathVariable Long id) {
        articleService.delete(id);
        return ResponseEntity.noContent().build();
    }
}
项目维护
定期备份网站数据，确保数据安全。
定期更新 Hexo 和主题，修复潜在的安全漏洞。
定期检查后台管理系统，确保功能正常运行。
贡献指南
欢迎任何有助于项目改进的贡献。
提交 Pull Request 前，请确保代码风格一致并经过测试。
联系方式
如果有任何问题或建议，请通过 GitHub Issues 提交。
希望这份 README 文档能够帮助你更好地理解和实现项目需求。如果有任何进一步的修改或补充，请随时告知。目中