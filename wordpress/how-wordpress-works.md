# How WordPress Works

## WordPress Fundamentals

Like other web development frameworks, WordPress follows the DRY principal (Don't Repeat Yourself). Although aspects of the WordPress dashboard are unique to WordPress, looking through the files of a WordPress theme, obvious similarities can be found between the logic and syntax of WordPress versus that of other web frameworks. 

### The Famous WordPress Loop

WordPress began as blogging software and many WordPress sites and themes have a blog-like appearance to them. At the core of WordPress is the famous "Loop." The Loops is the mechanism used to fetch posts and input them into the blog portion of the website, which is often the homepage. 

    <?php if ( have_posts() ) : ?>
        <?php while ( have_posts() ) : the_post(); ?>
            ... Display post content
        <?php endwhile; ?>
    <?php endif; ?>

### Templates

Similarities to other web development frameworks: 
* A WordPress theme can be as simple as an index.php and style.css file
* Styling is contained within CSS files. In WordPress, the primary CSS file is called style.css and is required to be stored in the theme root
* HTML templates are divided into partial files with a reference included in index.php, the primary file; common partial files are header.php, footer.php and sidebar.php. 
