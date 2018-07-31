# EventBus

�������ù۲���ģʽ��ƣ���һ������ά���۲��ߣ�Ȼ���¼������仯��ʱ��֪ͨ���еĹ۲��ߡ�

	public class EventBus {
	    // EventBus����ݱ�־
		private final String identifier;
		// ִ����
	    private final Executor executor;
	    // �쳣����
		private final SubscriberExceptionHandler exceptionHandler;
	    // ����ע���ȡ��ע��۲���
		private final SubscriberRegistry subscribers;
		// �¼��ַ�
		private final Dispatcher dispatcher;

		// ʵ���ڲ��������������췽������ʼ�����еı���
		EventBus(String identifier, Executor executor, Dispatcher dispatcher, SubscriberExceptionHandler exceptionHandler) {
			// ʹ��new�ؼ��ִ���һ��SubscriberRegistry���󣬲�����ǰ��EventBus��Ϊ��������
			this.subscribers = new SubscriberRegistry(this);
			this.identifier = (String)Preconditions.checkNotNull(identifier);
			this.executor = (Executor)Preconditions.checkNotNull(executor);
			this.dispatcher = (Dispatcher)Preconditions.checkNotNull(dispatcher);
			this.exceptionHandler = (SubscriberExceptionHandler)Preconditions.checkNotNull(exceptionHandler);
		}
	}
	
EventBusע���ȡ��ע��ķ���
	
    public void register(Object object) {
		// ʹ��SubscriberRegistry��ʵ������ע��
        this.subscribers.register(object);
    }

    public void unregister(Object object) {
		// ʹ��SubscriberRegistry��ʵ��ȡ��ע��
        this.subscribers.unregister(object);
    }

��ʹ��EventBus����һ��ֵ��ʱ����ڲ��߼���

    public void post(Object event) {
		// ��SubscriberRegistry�л�ȡ���еĹ۲���
        Iterator<Subscriber> eventSubscribers = this.subscribers.getSubscribers(event);
        if (eventSubscribers.hasNext()) {
			// ʹ��Dispatcher���зַ��¼�
            this.dispatcher.dispatch(event, eventSubscribers);
        } else if (!(event instanceof DeadEvent)) {
			// ���Ѿ������ڹ۲��ߵ�ʱ��ͷַ�һ��DeadEvent
            this.post(new DeadEvent(this, event));
        }
    }


	