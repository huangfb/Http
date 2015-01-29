# Http
封装了HTTP的网络解析，通过这个代码可以上传图片字符串，支持get和post方式。
调用的例子如下，创建一个NSObject类为逻辑层。* 编辑作者：
 * 版本编号：1.0.1
 * 完成日期：
 ***************************************************************************/

/***************************************************************************
 *                             文件引用
 ***************************************************************************/
#import "CRecruitmentLogic.h"
#import "CHttpDataDTO.h"
#import "CLocalCfg.h"
#import "CHttpCommDef.h"
#import "CHttpComm.h"
#import "CLocalCfg.h"
#import "CUserInfoDTO.h"

/***************************************************************************
 *                             宏定义
 ***************************************************************************/

/***************************************************************************
 *                             常量
 ***************************************************************************/

/***************************************************************************
 *                             类型定义
 ***************************************************************************/

/***************************************************************************
 *                             全局变量
 ***************************************************************************/
CRecruitmentLogic *g_recruitmentLogic = nil;

/***************************************************************************
 *                             原型
 ***************************************************************************/

/***************************************************************************
 *                             类特性
 ***************************************************************************/

@implementation CRecruitmentLogic

/***************************************************************************
 *                             类的实现
 ***************************************************************************/

/***************************************************************************
 * 方法名称：init
 * 功能描述：
 * 输入参数：
 * 输出参数：
 * 返回值：
 * 其它说明：
 ***************************************************************************/
- (id)init
{
    // 防止重复创建
    if (g_recruitmentLogic != nil)
    {
        return nil;
    }
    
    if ((self = [super init]) == nil)
    {
        return nil;
    }
    
    m_dataDTO = [[CHttpDataDTO alloc] init];
    
    g_recruitmentLogic = self;
    
    return self;
}

/***************************************************************************
 * 方法名称：instance
 * 功能描述：
 * 输入参数：
 * 输出参数：
 * 返回值：
 * 其它说明：
 ***************************************************************************/
+ (CRecruitmentLogic *)instance
{
    if (g_recruitmentLogic == nil)
    {
        NSLog(@"ERR instance nil");
    }
    
    return g_recruitmentLogic;
}

/***************************************************************************
 * 方法名称： preUrl
 * 功能描述： 获取url地址
 * 输入参数：
 * 输出参数：
 * 返回值：
 * 其它说明：
 ***************************************************************************/
- (NSString *)preUrl
{
    CLocalCfg *cfg = [CLocalCfg instance];
    NSString *url = [NSString stringWithFormat:@"%@", [cfg httpServerAddr]];
    return url;
}

/***************************************************************************
 * 方法名称： builderJsonForBody
 * 功能描述： 拼接成json
 * 输入参数：
 * 输出参数：
 * 返回值：
 * 其它说明：
 ***************************************************************************/
- (NSString *)builderJsonForBody:(NSDictionary *)dicData
{
    NSMutableString *body = [[NSMutableString alloc] initWithString:@"{"] ;
    NSArray *keys = [dicData allKeys];
    for (NSString *key in keys)
    {
        NSString *value = [dicData objectForKey:key];
        if (value == nil)
        {
            // 防止出现null字符串
            value = @"";
        }
        [body appendFormat:@"\"%@\":\"%@\",", key, value];
    }
    NSString *newBody = [body substringToIndex:body.length - 1];
    NSMutableString *returnBody = [NSMutableString stringWithString:newBody];
    [returnBody appendString:@"}"];
    return returnBody;
}

/***************************************************************************
 * 方法名称： builderStringForBody
 * 功能描述： 拼接成"&xxxx=yyyy"形式
 * 输入参数：
 * 输出参数：
 * 返回值：
 * 其它说明：
 ***************************************************************************/
- (NSString *)builderStringForBody:(NSDictionary *)dicData
{
    NSMutableString *body = [[NSMutableString alloc] init] ;
    NSArray *keys = [dicData allKeys];
    for (NSString *key in keys)
    {
        NSString *value = [dicData objectForKey:key];
        [body appendFormat:@"%@=%@&", key, value];
    }
    NSString *newBody = [body substringToIndex:body.length - 1];
    NSMutableString *returnBody = [NSMutableString stringWithString:newBody];
    return returnBody;
}

/***************************************************************************
 * 方法名称： getJobListRoot:sender:sel:
 * 功能描述： 逻辑层获取招聘信息列表
 * 输入参数：
 * 输出参数：
 * 返回值：
 * 其它说明：
 ***************************************************************************/
- (void)getJobListRoot:(NSDictionary *)reInfo sender:(id)sender sel:(SEL)sel
{
    NSString *url = [NSString stringWithFormat:@"%@/%@%@", [self preUrl], @"api/job?",[self builderStringForBody:reInfo]];
    [[CHttpComm instance] asynGetString:url sender:sender sel:sel];
    
}

创建plist如图
Key                                Type                 Value
Root                               Dictonary            (7 items)
  HttpServerAddr                   String               http://IP
  HttpServerAddrFixString          String
  HttpServerImageAddr              String
  HttpExpiredTime                  String                20
  HttpRequestDataCodeFormatType    String
  HttpResponseDataCodeFormatType   String
  HttplsUseLocalSimulateData       String          
具体看demo
