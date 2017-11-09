## MicroService.Core <a name="top" href="#top"></a>

> `MicroService.Core` �ĳ�����Ϊ�˷���Ĵ���һ��΢����
> ����Ϊ Windows Service ���߿���̨ģʽ������
> ���ײ�ʹ���� Nancy��ʹ�ÿ������̺ܼ򵥣��������
> 
><b style='color:red'>ע�⣺���°汾������ OWIN ��</b>

* [��������](#quick-start)
* [��ܹ���](#principle)
* [����](#advance)
* [��չ](#extensions)
* [ʵ��](#samples)

## <a name="quick-start" href="#quick-start">��������</a>
#### һ����������̨��Ŀ����Ҫ.net 4.5���ϣ�
#### ������װNuget��
```
PM> Install-Package MicroService.Core -Version 1.2
```
���� Nuget �������������� `MicroService.Core` ��װ��
#### ������д Program.cs���������´���
������ã�`using MicroService.Core;`
Main ������д���´��룺
```
var service = new MicroServiceBase("MyService");
service.Run(args);
```
��ʱ Program.cs �ļ���������
```
using MicroService.Core;
namespace MicroService.Samples
{
    class Program
    {
        static void Main(string[] args)
        {
            var service = new MicroServiceBase("MyService");
            service.Run(args);
        }
    }
}
```
������Ŀ��Ȼ�������ɵ� EXE �ļ�Ŀ¼�´�һ�������д��ڣ�ִ�У�
> �����ɵ� EXE �ļ�Ϊ `MicroService.Samples.exe`
###### 1��������ģʽ���У�
```
MicroService.Samples.exe --console
```
���ɿ������µ�����Ч����
![Sample1](http://res.mrhuo.com/github/microservice-sample1.png)

###### 2��Windows ����ģʽ
```
MicroService.Samples.exe --install
```
���н���������������������
![Sample2](http://res.mrhuo.com/github/microservice-sample2.png)
����ΪȨ�޲������޷���װwindows�����Թ���ԱȨ�����������д����ٴ�ִ���������
��ͼ���£�
![Sample3](http://res.mrhuo.com/github/microservice-sample3.png)
�鿴 `Windows ����` �б����Կ����˷����Ѿ��������У�
![Sample4](http://res.mrhuo.com/github/microservice-sample4.png)
#### �ġ���β��Դ˷����Ƿ��Ѿ��������У�
��΢������Ĭ���ṩ�˷���״̬��ַ `/health`����ͼ��Ĭ�ϰ󶨵�ַ�� 
`http://127.0.0.1:8080`������������ʵ�ַ 
[http://127.0.0.1:8080/health](http://127.0.0.1:8080/health)
���Եõ����µ������
![Sample5](http://res.mrhuo.com/github/microservice-sample5.png)
���Կ��������һЩ���������ʱ������¶�˵�ȵ�...
#### �塢�Ƿ񱻾�����
����3�д��룬��ȥ using ���ã���ֻ��2�д��룬�Ϳ�����һ������������������
/windows ����Ķ����ṩ `API` �����ĳ���

## <a name="principle" href="#principle">��ܹ���</a>
* ͨ������ Owin ʹ�����������йܵ�����
* ͨ��ʵ����һ���ڲ��� ServiceBase ��ʹ������������Ϊ windows �������е�����
* ͨ������ Nancy ��[http://nancyfx.org/](http://nancyfx.org/)�� ʹ�������˺����׿��� Api ���������

> ���˿��ܻ�˵�����и����ã��ȱ𼱣���������չһ�£�����ʲô�أ�
> ���������ӷ����ɣ������������֣�������������֮�͡�

## <a name="advance" href="#advance">����</a>
#### һ��дһ�� Nancy ģ�飺
> ���� Nancy ģ��ı�д��������ǰ������ [http://nancyfx.org/](http://nancyfx.org/)
> ѧϰ����Ϊ������Ա��ѧϰ�Ǳز����ٵļ��ܡ���Ȼ���㿴������Ȳ����ż�ȥѧϰ Nancy��
> �������¿����ܼ򵥵ģ�

###### 1������һ����
����Ŀ���½�һ����Ϊ `AddModule` �� Nancy ģ�飬�������£�
```
using Nancy;
using System.Threading.Tasks;
namespace MicroService.Samples
{
    public class AddModule : NancyModule
    {
        public AddModule()
        {
            Get["/add", true] = async (_, ctx) =>
            {
                return await Task.Run(() =>
                {
                    int? num1 = Request.Query.num1;
                    int? num2 = Request.Query.num2;
                    if (num1.HasValue && num2.HasValue)
                    {
                        return $"{num1} + {num2} = {num1 + num2}";
                    }
                    return "Paramters num1 and num2 missing!";
                });
            };
        }
    }
}
```
��δ�����ܶ�û�� Nancy ģ�龭����ˣ���΢�е��Ѷȣ����Ǵ���ͨ���׶���һ���ͻᣡ

> ͣ���������е� Windows ����Ȼ������������Ŀ����������

�����������[http://127.0.0.1:8080/add?num1=1&num2=2](http://127.0.0.1:8080/add?num1=1&num2=2)
![Sample6](http://res.mrhuo.com/github/microservice-sample6.png)

����������⣬������н��Ӧ�ú������죡

> ������������е��뷨�ˣ����Ҳ����� VS ������
> ���ǣ����ԣ�

*�� VS ��Ŀ���Ҽ���������Ŀ����ҳ�棬�ڵ���ѡ��µ��������������� --console��Ȼ��������Ŀ*
��ͼ��
![Sample7](http://res.mrhuo.com/github/microservice-sample7.png)

�������¿�������̨������Ч����
![Sample8](http://res.mrhuo.com/github/microservice-sample8.png)

����ɫ��ͷ�������������У����ظո��½���ģ��ɹ���
> ����¶��URL���г����ˣ��Ƿ�����ģ�

�������Ҹо�����û�����ֳ��������ƣ������ٽ������Ŀ������

###### 2���ٵ����½�һ���� RedisService.cs
�������£�
```
using System;
namespace MicroService.Samples
{
    public class RedisService
    {
        public string RedisServiceStatus
        {
            get
            {
                var rnd = new Random().Next(1, 10);
                if (rnd % 3 == 0)
                {
                    return "DOWN";
                }
                return "UP";
            }
        }
    }
}
```
����ܼ򵥣�ֻ��һ��ֻ�����ԣ���ȡ�� Redis �����״̬��ģ�⣩��
���棬���ǰ������뵽����� `/health` �˵��У������ǵķ������ֱ�۵�չʾ����������״̬��

�޸� Program.cs ���еĴ��룺
```
using MicroService.Core;
using Nancy.TinyIoc;
namespace MicroService.Samples
{
    class Program
    {
        static void Main(string[] args)
        {
            var service = new MicroServiceBase("MyService");
            service.OnServiceStarting += Service_OnServiceStarting;
            service.OnServiceStatusUpdating += Service_OnServiceStatusUpdating;
            service.Run(args);
        }
        /// <summary>
        /// ��������֮ǰִ���¼�
        /// </summary>
        /// <param name="service"></param>
        /// <param name="container"></param>
        private static void Service_OnServiceStarting(
            MicroServiceBase service, 
            TinyIoCContainer container)
        {
            var redisService = new RedisService();
            container.Register(redisService);
        }
        /// <summary>
        /// ����״̬�����¼�
        /// </summary>
        /// <param name="service"></param>
        /// <param name="serviceStatus"></param>
        private static void Service_OnServiceStatusUpdating(
            MicroServiceBase service, 
            ServiceStatus serviceStatus)
        {
            var redisService = service.GetComponent<RedisService>();
            if (redisService != null)
            {
                serviceStatus.AddOrUpdate("RedisStatus", redisService.RedisServiceStatus);
            }
        }
    }
}
```

> ���˿���Ҫ�����ˣ��� TM ���Ұ�����м򵥣���ʵ��ϸ�������ѣ�

���������¼�����������֮ǰִ���¼��ͷ���״̬�����¼����ڷ�������֮ǰִ���¼������У�
ʹ�� Nancy ���õ� TinyIoCContainer IoC����ע����һ�� RedisService ��ʵ����
�ڷ���״̬�����¼��У�ͨ�� `service.GetComponent<RedisService>()` ��ȡ�������
RedisService ʵ������ serviceStatus ��ӽ�ȥ��

���ڣ��ٴ����У����������
![Sample9](http://res.mrhuo.com/github/microservice-sample9.png)

## <a name="extensions" href="#extensions">��չ</a>
> ��������

## <a name="samples" href="#samples">ʵ��</a>
> ��������

> QQ: 491217650  �ʼ���admin@mrhuo.com

<a href="#top">�ص�����</a>