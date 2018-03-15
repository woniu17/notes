# 配置解析机制
## 命令结构体
```
struct ngx_command_s {
    ngx_str_t             name; // 命令名称
    ngx_uint_t            type; // 命令类型
    char               *(*set)(ngx_conf_t *cf, ngx_command_t *cmd, void *conf);
    ngx_uint_t            conf;
    ngx_uint_t            offset;
    void                 *post;
};
typedef struct ngx_command_s         ngx_command_t;
```
## 命令类型
```
/*
 *        AAAA  number of arguments
 *      FF      command flags
 *    TT        command type, i.e. HTTP "location" or "server" command
 */

#define NGX_CONF_NOARGS      0x00000001 // 无配置参数
#define NGX_CONF_TAKE1       0x00000002 // 配置参数个数为1个
#define NGX_CONF_TAKE2       0x00000004 // 配置参数个数为2个
#define NGX_CONF_TAKE3       0x00000008 // 配置参数个数为3个
#define NGX_CONF_TAKE4       0x00000010 // 配置参数个数为4个
#define NGX_CONF_TAKE5       0x00000020 // 配置参数个数为5个
#define NGX_CONF_TAKE6       0x00000040 // 配置参数个数为6个
#define NGX_CONF_TAKE7       0x00000080 // 配置参数个数为7个

#define NGX_CONF_MAX_ARGS    8 // 如果配置项的类型不是NGX_CONF_ANY，那么该配置项的配置参数个数限制在7个（扣除配置项名称）

#define NGX_CONF_TAKE12      (NGX_CONF_TAKE1|NGX_CONF_TAKE2) // 配置参数个数为1个或2个
#define NGX_CONF_TAKE13      (NGX_CONF_TAKE1|NGX_CONF_TAKE3) // 配置参数个数为1个或3个
#define NGX_CONF_TAKE23      (NGX_CONF_TAKE2|NGX_CONF_TAKE3) // 配置参数个数为2个或3个
#define NGX_CONF_TAKE123     (NGX_CONF_TAKE1|NGX_CONF_TAKE2|NGX_CONF_TAKE3) // 配置参数个数为1个或2个或3个
#define NGX_CONF_TAKE1234    (NGX_CONF_TAKE1|NGX_CONF_TAKE2|NGX_CONF_TAKE3   \
                              |NGX_CONF_TAKE4) // 配置参数个数为1个或2个或3个或4个

#define NGX_CONF_ARGS_NUMBER 0x000000ff // 该值暂时没有用到
#define NGX_CONF_BLOCK       0x00000100 // 块配置，用大括号包围{}
#define NGX_CONF_FLAG        0x00000200 // 开关配置，on/off
#define NGX_CONF_ANY         0x00000400 // 该类型的配置项，配置参数个数不限（目前无该类型配置项）
#define NGX_CONF_1MORE       0x00000800 // 配置参数个数大于1个
#define NGX_CONF_2MORE       0x00001000 // 配置参数个数大于2个

#define NGX_DIRECT_CONF      0x00010000 // 

#define NGX_MAIN_CONF        0x01000000
#define NGX_ANY_CONF         0x1F000000 // 任意类型的配置项，目前只有include配置项使用


#define NGX_HTTP_MAIN_CONF        0x02000000
#define NGX_HTTP_SRV_CONF         0x04000000
#define NGX_HTTP_LOC_CONF         0x08000000
#define NGX_HTTP_UPS_CONF         0x10000000
#define NGX_HTTP_SIF_CONF         0x20000000
#define NGX_HTTP_LIF_CONF         0x40000000
#define NGX_HTTP_LMT_CONF         0x80000000


#define NGX_EVENT_CONF        0x02000000

```
1. NGX_DIRECT_CONF和NGX_MAIN_CONF有何区别
1. 为什么配置项既可以属于NGX_DIRECT_CONF，又可以属于NGX_MAIN_CONF
1. 配置在server{}里面的error_log为什么会生效
    ```
    # error_log的类型为 NGX_MAIN_CONF|NGX_CONF_1MORE
    # ngx_http_core_server()
    cf->cmd_type = NGX_HTTP_SRV_CONF;
    # ngx_conf_parse()->ngx_conf_handler()
    if (!(cmd->type & cf->cmd_type)) {
                continue;
    }
    ```
```
static ngx_core_module_t  ngx_core_module_ctx = {
    ngx_string("core"),
    ngx_core_module_create_conf,
    ngx_core_module_init_conf
};
static ngx_core_module_t  ngx_errlog_module_ctx = {
    ngx_string("errlog"),
    NULL,
    NULL
};
static ngx_core_module_t  ngx_regex_module_ctx = {
    ngx_string("regex"),
    ngx_regex_create_conf,
    ngx_regex_init_conf
};
static ngx_core_module_t  ngx_thread_pool_module_ctx = {
    ngx_string("thread_pool"),
    ngx_thread_pool_create_conf;
    ngx_thread_pool_init_conf;
};
static ngx_core_module_t  ngx_events_module_ctx = {
    ngx_string("events"),
    NULL,
    ngx_event_init_conf
}
static ngx_core_module_t  ngx_openssl_module_ctx = {
    ngx_string("openssl"),
    ngx_openssl_create_conf,
    NULL
}; 
static ngx_core_module_t  ngx_http_module_ctx = {
    ngx_string("http"),
    NULL,
    NULL
};
static ngx_core_module_t  ngx_mail_module_ctx = {
    ngx_string("mail"),
    NULL,
    NULL
};
static ngx_core_module_t  ngx_google_perftools_module_ctx = {
    ngx_string("google_perftools"),
    ngx_google_perftools_create_conf,
    NULL
};
static ngx_core_module_t  ngx_stream_module_ctx = {
    ngx_string("stream"),
    NULL,
    NULL
};
```