��20200618��
��20200612�����ϣ������˰�ƽ������/��ÿ���е�Job�������еĹ��ܣ�Ŀ���Ǽ����̴߳���Jobʱ��cache/DRAM bound��
��Job���ͱ�Ϊ���֣�
- JOBALLOCATION_INTERLEAVED_JOB		//20200612�е�ALL-EVEN��ÿ���߳�Ϊ�˸��ؾ��⣬���̶������job queue��ȡJob
- JOBALLOCATION_SEQUENTIAL_JOB		//���������ͣ�ÿ���̴߳�job queue��ȡһ��������job
- JOBALLOCATION_JOB_SHARING		//20200612�е�ALL-Stealing����ʵ������Ϊjob-sharing��job-stealing��ȷ�С�

ʵ�������
�̴߳���������Job���ȴ����������Job��ȷʵmemory bound�м��١�


��20200612��
��20200610�����ϣ������̸߳ĳ���ȾJob��deferredcontext��������ԭ�ȵ�immediate context���С����̺߳͹����̶߳�����ͬһ��BuildCommandList������

ʵ�������
1.���̸߳�Ϊ��Ⱦ��deferredcontext�У��͹����߳�һ������ȷʵ�����������̵߳ȴ������̵߳ĸ��ʡ�
2.��Job����Ŀ����Ƚϴ�������ALL_EVENģʽ����ALL_Stealingģʽ��FPS�������½�����Σ���ʹ��ALL_EVENģʽ��job����Ҳû���𵽸��ؾ�������ã�GPUView��ʾ��û������ĸ��ط��䡣


��20200610��
��20200606�����ϣ����Ӷ�job������������job�ȴ���Ŀ���ǽ�һ����߶��̸߳��ؾ����ԣ��������̵߳ȴ��ĸ��ʡ�
���ַ����ɱ���ĳ���̴߳������һ������������������̡߳�����������work stealing Job��ģʽ���������ھ�̬����Job��ģʽ��

����ʹ��stealingģʽ�������п��ܳ������̵߳ȴ������̵߳��������Ҫԭ���ǣ������߳����Ҫִ��finish command list�������������������Job������ʱ��Job��ܶ࣬�����߳�ִ����Job�󣬲���Ҫִ�д˲�����

�ο���
https://www.cnblogs.com/luorende/p/6121906.html
https://blog.csdn.net/zhangpiu/article/details/50564064


��20200606��
��20200602�汾�Ļ�����ʵ�ִ˰汾���Ĵ�immediate��ȾΪ���߳�deferred��Ⱦ������0602�汾������ƿ�����⡣���ڱȽϾ�̬���ֺͶ�̬Stealingģʽ�ھ���ƽ̨�ϵ����ܡ�

ʵ��2��ģʽ��
1. ��̬������ͬ����JOB�����̺߳͹����̡߳�
����JOB����ͬһ�������У����߳����߳�����ΪStep�Ӷ����л�ȡJob��ʵ�־�̬�ĸ��ؾ��⡣
2. ���̺߳͹����߳�ͬʱStealing Job��

JOB������Ⱦdraw calls��

�����߳�����Ϊ�������-2����Ϊ���߳���busy�ģ���Ҫ��ȥ�����̡߳�

CDXUTSDKMesh::RenderMesh()�б������º�����
    if( 0 < GetOutstandingBufferResources() )
        return;

