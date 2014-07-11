# Wordpress 常用函数

## Header 中常用函数

```php
<?php bloginfo('name'); ?> : 博客名称(Title)
<?php bloginfo('stylesheet_url'); ?> : CSS文件路径
<?php bloginfo('template_url'); ?> : 模板文件路径
<?php bloginfo('version'); ?> : WordPress版本
<?php bloginfo('url'); ?> : 博客 Url
<?php bloginfo('html_type'); ?> : 博客网页Html类型
<?php bloginfo('charset'); ?> : 博客网页编码
<?php bloginfo('description'); ?> : 博客描述
<?php wp_title(); ?> : 特定内容页(Post/Page)的标题

<!-- 根据内容显示不同 title -->
<title><?php wp_title(' '); ?> <?php if(wp_title(' ', false)) { echo ' - '; } ?><?php bloginfo('name'); ?></title>
```

## 模板中常用函数

```php
<?php get_header(); ?> : 调用Header模板
<?php get_sidebar(); ?> : 调用Sidebar模板
<?php get_footer(); ?> : 调用Footer模板
<?php the_content(); ?> : 显示内容(Post/Page)
<?php if(have_posts()) : ?> : 检查是否存在Post/Page
<?php while(have_posts()) : the_post(); ?> : 如果存在Post/Page则予以显示
<?php endwhile; ?> : While 结束
<?php endif; ?> : If 结束
<?php the_time('字符串') ?> : 显示时间，时间格式由“字符串”参数决定，具体参考PHP手册
<?php comments_popup_link(); ?> : 正文中的留言链接。如果使用 comments_popup_script() ，则留言会在新窗口中打开，反之，则在当前窗口打开
<?php the_title(); ?> : 内容页(Post/Page)标题
<?php the_permalink() ?> : 内容页(Post/Page) Url
<?php the_category('') ?> : 特定内容页(Post/Page)所属Category
<?php the_author(); ?> : 作者
<?php the_ID(); ?> : 特定内容页(Post/Page) ID
<?php edit_post_link(); ?> : 如果用户已登录并具有权限，显示编辑链接
<?php get_links_list(); ?> : 显示Blogroll中的链接
<?php comments_template(); ?> : 调用留言/回复模板
<?php wp_list_pages(); ?> : 显示Page列表
<?php wp_list_categories(); ?> : 显示Categories列表
<?php next_post_link(' %link '); ?> : 下一篇文章链接
<?php previous_post_link('%link'); ?> : 上一篇文章链接
<?php get_calendar(); ?> : 日历
<?php wp_get_archives() ?> : 显示内容存档
<?php posts_nav_link(); ?> : 导航，显示上一篇/下一篇文章链接
<?php include(TEMPLATEPATH . '/文件名'); ?> : 嵌入其他文件，可为定制的模板或其他类型文件
<?php _e('Message'); ?> : 输出相应信息
<?php wp_register(); ?> : 显示注册链接
<?php wp_loginout(); ?> : 显示登录/注销链接
```

## 功能片段

**搜索框**

```php
<form action="<?php bloginfo('home') ?>" method="get" class="dib-wrap">
    <input name="s" type="text" class="dib" placeholder="请输入搜索关键词!" value="<?php the_search_query() ?>">
    <input class="submit dib" type="submit" value="" />
</form>
```

**文章循环**

```php
<?php while ( have_posts() ) : the_post(); ?>
    <div class="entry-item">
        <h4><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>
        </h4>
        <div class="entry-meta"><?php the_time('n月j日 G:H'); ?></div>
        <div class="entry-content">
            <?php the_excerpt('Read the rest &raquo;'); ?>
        </div>
    </div>
<?php endwhile; ?>
```

**调用指定分类文章**

```php
<?php query_posts('cat=1&showposts=1'); ?>
<?php while ( have_posts() ) : the_post(); ?>
    <div class="entry-item">
        <h4><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>
        </h4>
        <div class="entry-meta"><?php the_time('n月j日 G:H'); ?></div>
        <div class="entry-content">
            <?php the_excerpt('Read the rest &raquo;'); ?>
        </div>
    </div>
<?php endwhile; ?>
```

**最新文章**

```php
<ul class="hot-list">
    <?php 
        $post_query = new WP_Query('showposts=5');
        while ($post_query->have_posts()) : $post_query->the_post();
        $do_not_duplicate = $post->ID; 
    ?>  
        <li><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></li>
    <?php endwhile;?>
</ul>
```

**随机文章**

```php
<?php
    $rand_posts = get_posts('numberposts=10&orderby=rand');
    foreach( $rand_posts as $post ) :
?>
    <!–下面是你想自定义的Loop–>
    <li><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></li>
<?php endforeach; ?>
```

**指定分类显示文章数**

```php
<?php $posts = query_posts($query_string . '&orderby=date&showposts=10'); ?>
```

**调用指定自定义字段**

```php
<?php echo get_post_meta($post->ID, "品牌", $single = true); ?>
```

**不同分类不同 single 页**

```php
<?php 
if ( in_category(3)) {
    get_template_part('single3' );
} elseif ( in_category(1)) {
    get_template_part('single2' );
} else {
    get_template_part('single1' );
}
?>
```

