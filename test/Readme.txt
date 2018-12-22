���:
	LSM303DLHC��������������ɣ��ֱ���acceleration��magnetometer.
	Acceleration�Ľӿڻ�·���ǣ�LSM303DLHC_ACC_PATH_BASE�� ��ֵΪ"/sys/devices/virtual/input/input2/"
	Magnetometer�Ľӿڻ�·���ǣ�#define LSM303DLHC_MAG_PATH_BASE, ��ֵΪ"/sys/devices/virtual/input/input3/"
	
Ĭ������:
	Acceleration:
		1. ��Դģʽ������
		2. Z/Y/X������
		3. ���������շ�׼��״̬��INT1
		4. BDUΪ1 (������ݲ�����, ֱ��MSB��LSB����)
		5. ����RateΪ1344HZ(�ڵ͵�ģʽ��Ϊ400HZ)
		6. ����RangeΪ8G
		7. HR����(�߷ֱ������ģʽ����)
	Magnetometer:
		1. �¶ȴ���������
		2. ����RateΪ75HZ
		3. RangeΪ1.9G
		4. ������ģʽΪContinuous-conversionģʽ
	
���ú�:
	���� #include "lsm303dlhc.h"

	#define LSM303DLHC_ACC_PATH_BASE				Acceleration��·��	
	#define LSM303DLHC_MAG_PATH_BASE				Magnetometer��·��

	ע�⣺
		Acceleration���������Ƶ�ʷ�Ϊ��ͨģʽ�͵͵�ģʽ�� ��ͨģʽ��Ƶ�ʹ̶�λ1344HZ
		�͵�ģʽ��Ϊ�����(1HZ, 10HZ, 25HZ, ..., 5376HZ).
		�͵�ģʽ�����÷���Ϊ��lowpowerΪ����״̬��HRΪ��ֹ״̬
		��ͨģʽ�����÷���Ϊ��lowpowerΪ����״̬��HRΪ����״̬
		�͵�ģʽֻ��Ҫ����lowpower, ���л��Զ�����HR��״̬�������ֶ�����
		
	/****************************** Acceleration ****************************************/
	#define LSM303DLHC_ACC_POWERDOWN				���õ���ģʽ
	#define LSM303DLHC_ACC_RATE_1HZ					����ģʽ�µ�Ƶ��Ϊ1HZ
	#define LSM303DLHC_ACC_RATE_10HZ				����ģʽ�µ�Ƶ��Ϊ10HZ
	#define LSM303DLHC_ACC_RATE_25HZ				����ģʽ�µ�Ƶ��Ϊ25HZ	
	#define LSM303DLHC_ACC_RATE_50HZ				����ģʽ�µ�Ƶ��Ϊ50HZ
	#define LSM303DLHC_ACC_RATE_100HZ				����ģʽ�µ�Ƶ��Ϊ100HZ
	#define LSM303DLHC_ACC_RATE_200HZ				����ģʽ�µ�Ƶ��Ϊ200HZ
	#define LSM303DLHC_ACC_RATE_400HZ				����ģʽ�µ�Ƶ��Ϊ400HZ
	#define LSM303DLHC_ACC_RATE_1620HZ				����ģʽ�µ�Ƶ��Ϊ1620HZ
	#define LSM303DLHC_ACC_RATE_5376HZ				����ģʽ�µ�Ƶ��Ϊ5376HZ

	#define LSM303DLHC_ACC_RATE_1344HZ				��ͨģʽ�µ�Ƶ��Ϊ1344HZ

	#define LSM303DLHC_ACC_LOWPOWER_DIS				���õ͵�ģʽ
	#define LSM303DLHC_ACC_LOWPOWER_EN				���õ͵�ģʽ

	#define LSM303DLHC_ACC_ZYZ_DIS					Z/Y/X�����
	#define LSM303DLHC_ACC_ZYZ_EN					Z/Y/X������

	#define LSM303DLHC_ACC_HIGHPASS_MODE_NOR_REST	��ͨ�˲�ģʽΪ����ģʽ(����)
	#define LSM303DLHC_ACC_HIGHPASS_MODE_REF		��ͨ�˲�ģʽΪ�ο��ź��˲�ģʽ
	#define LSM303DLHC_ACC_HIGHPASS_MODE_NOR		��ͨ�˲�ģʽΪ����ģʽ
	#define LSM303DLHC_ACC_HIGHPASS_MODE_AUTO		��ͨ�˲�ģʽΪ���ж�ʱ��ʱ�Զ�����

	#define LSM303DLHC_ACC_FILTER_BYPASS			��������ѡ��Ϊ�ڲ���������·
	#define LSM303DLHC_ACC_FILTER_OUTPUT			��������ѡ��Ϊ���ݴ��ڲ��˲������͵�����Ĵ�����FIFO

	#define LSM303DLHC_ACC_HIGHPASS_CLICK_BYPASS	��ͨ�˲�����Ϊ��CLICK�Ĺ�������·
	#define LSM303DLHC_ACC_HIGHPASS_CLICK_EN		��ͨ�˲�����Ϊ��CLICK�Ĺ��˹���

	#define LSM303DLHC_ACC_HIGHPASS_IRQ2_BYPASS		��IRQ2��AOI�������ø�ͨ�˲�������·����
	#define LSM303DLHC_ACC_HIGHPASS_IRQ2_EN			��IRQ2��AOI�������ø�ͨ�˲�

	#define LSM303DLHC_ACC_HIGHPASS_IRQ1_BYPASS		��IRQ1��AOI�������ø�ͨ�˲�������·����
	#define LSM303DLHC_ACC_HIGHPASS_IRQ1_EN			��IRQ1��AOI�������ø�ͨ�˲�

	#define LSM303DLHC_ACC_BDU_UPDATE				���ݿ���������
	#define LSM303DLHC_ACC_BDU_NOT_UPDATE			���ݿ�������Ĵ��������£� ֱ��MSB��LSM����

	#define LSM303DLHC_ACC_BLE_LSB					LSBģʽ
	#define LSM303DLHC_ACC_BLE_MSB					MSBģʽ

	#define LSM303DLHC_ACC_RANGE_2G					������ѡ��2G
	#define LSM303DLHC_ACC_RANGE_4G					������ѡ��4G
	#define LSM303DLHC_ACC_RANGE_8G					������ѡ��8G
	#define LSM303DLHC_ACC_RANGE_16G				������ѡ��16G
	
	#define LSM303DLHC_ACC_HR_DIS					�߷ֱ������ģʽ����
	#define LSM303DLHC_ACC_HR_EN					�߷ֱ������ģʽ����

	#define LSM303DLHC_ACC_REBOOT_NOR				�ڴ�����Ϊ����ģʽ
	#define LSM303DLHC_ACC_REBOOT_EN				���������ڴ�����

	#define LSM303DLHC_ACC_FIFO_DIS					FIFO��ֹ
	#define LSM303DLHC_ACC_FIFO_EN					FIFO����

	#define LSM303DLHC_ACC_NO_XYZDATA_OVERRUN		X/Y/Z������û�����
	#define LSM303DLHC_ACC_HAVE_XYZDATA_OVERRUN		X/Y/Z���������

	#define LSM303DLHC_ACC_NO_ZYXDATA_AVAILABLE		X/Y/Z��û���µĿ�������
	#define LSM303DLHC_ACC_HAVA_ZYXDATA_AVAILABLE	X/Y/Z�����µĿ��õ�����

	#define LSM303DLHC_ACC_FIFO_BYPASS_MODE			FIFOģʽΪ��·ģʽ
	#define LSM303DLHC_ACC_FIFO_FIFO_MODE			FIFOģʽΪFIFOģʽ
	#define LSM303DLHC_ACC_FIFO_STREAM_MODE			FIFOģʽΪ��ģʽ
	#define LSM303DLHC_ACC_FIFO_TRIGGER_MODE		FIFOģʽΪ����ģʽ
	
�����ӿ�:
	reg					�Ĵ����ӿ�, �ɲ鿴���мĴ�����ֵ���޸��ض��Ĵ�����ֵ
						���������ʽ: "0x%02x:0x%02x" 				(����: "0x02:0xef")
						read(L3G_GET_PATH(reg), buf, 100);			��ȡȫ���Ĵ�����ֵ
																	����ֵ֮���Կո�ָ�(ע��: buf����̫С)
						write(L3G_GET_PATH(reg), "0x02:0xef", 10);	�޸ļĴ���0x02ֵΪ0xef 
	
	delay				�ӳٽӿ�				
						���������ʽ: "%d"							(����: "50")
			
	position			������ٶ�ƫ��ֵ
						���������ʽ: "%d", Ĭ��ֵΪ-3
	
	fuzz				δ֪
	
	enable				������ٶȵ�Դ����
						���������ʽ: "%d", �������ֵΪ(0000 - 1001)
						0000: �ϵ�, ����ֵ: ����
	
	sample				��ȡ/���������������
						���������ʽ: "%d", �������ֵΪ(95HZ, 190HZ, 380HZ, 760HZ)	
		
	lowpower			��ȡ/���õ͵�ģʽ
						���������ʽ: "%d", �������ֵΪ(0-1)
						0: ��ͨģʽ, 1: �͵�ģʽ
						
	axis				����/����X/Y/Z��
						���������ʽ�� "%d", �������ֵΪ(0-7)
						0: X/Y/Zȫ����ֹ, 1: X��ʹ��, 2: Y��ʹ��, 4: Z��ʹ�� (����ʹ�û�����)
	
	highpass_mode		��ȡ/���ø�ͨ�˲�ģʽѡ��
						���������ʽ: "%d",  �������ֵΪ(0-3) 
						0: һ��ģʽ(����), 1: �ο��˲��ź�, 2: ��ͨģʽ, 3: �ж��¼��Զ�����
						
	highpass_freq		��ȡ/���ø�ͨ�˲�Ƶ��
	
	filter				��ȡ/���ù�������ѡ��
						���������ʽ��"%d", �������ֵΪ(0-1)
						0: �ڲ���������·, 1: ���ݴ��ڲ��˲������͵�����Ĵ�����FIFO
	
	highpass_click		��ȡ/����������CLICK���ܵĸ�ͨ�˲���
						���������ʽ�� "%d", �������ֵΪ(0-1)
						0: ��������·, 1: ����������
	
	highpass_irq2		��ȡ/���ö��ж�2��AOI�������ø�ͨ�˲���
						���������ʽ: "%d", �������ֵΪ(0-1)
						0: ��������·, 1: ����������
	
	highpass_irq1		��ȡ/���ö��ж�1��AOI�������ø�ͨ�˲���
						���������ʽ: "%d", �������ֵΪ(0-1)
						0: ��������·, 1: ����������
	
	bdu					��ȡ/���ÿ����ݸ���
						���������ʽ: "%d",  �������ֵΪ(0-1)
						0�� ��������, 1: ����Ĵ���������
	
	ble					��ȡ/���ô�С������ѡ��
						���������ʽ: "%d", �������ֵΪ(0-1)
						0: С��, 		1: ���
	
	range				��ȡ/��������ѡ��
						���������ʽ�� "%d", �������ֵΪ(2G, 4G, 8G, 16G)
	
	hr					��ȡ/���ø߷ֱ������ģʽ
						���������ʽ: "%d", �������ֵΪ(0-1)
						0: ��ֹ,		1: ����
	
	boot				��ȡ/���������ڴ�����״̬
						���������ʽ: "%d", �������ֵΪ(0-1)
						0: ����ģʽ, 	1: �����ڴ�����
	
	fifo_en				���/����FIFOʹ��
						���������ʽ: "%d",  �������ֵΪ(0-1)
						0: ��ֹ, 1: ʹ��
	
	zyxor				��ȡX/Y/Z�������Ƿ����
						�����ʽ: "%d",  ���ֵΪ(0-1)
						0: û���������, 1: ���
	
	zor					��ȡZ�������Ƿ����
						�����ʽ: "%d",  ���ֵΪ(0-1)
						0: û���������, 1: ���
	
	yor					��ȡY�������Ƿ����
						�����ʽ: "%d",  ���ֵΪ(0-1)
						0: û���������, 1: ���
						
	xor					��ȡX�������Ƿ����
						�����ʽ: "%d",  ���ֵΪ(0-1)
						0: û���������, 1: ���
						
	zyxda				��ȡX/Y/Z�������Ƿ��������ݿ���
						�����ʽ: "%d",  ���ֵΪ(0-1)
						0: û�п�������, 1: ��
						
	zda					��ȡZ�������Ƿ��������ݿ���
						�����ʽ: "%d",  ���ֵΪ(0-1)
						0: û�п�������, 1: ��
						
	yda					��ȡY�������Ƿ��������ݿ���
						�����ʽ: "%d",  ���ֵΪ(0-1)
						0: û�п�������, 1: ��
						
	xda					��ȡX�������Ƿ��������ݿ���
						�����ʽ: "%d",  ���ֵΪ(0-1)
						0: û�п�������, 1: ��
	
	acc					��ȡ������ٶȵ�����ֵ
						�����ʽ: "%d %d %d"
		
	fifo_mode			��ȡ/����FIFOģʽ
						���������ʽ: "%d", �������ֵΪ(0-3)
						0: Bypass mode, 1: FIFO mode, 2: Stream mode 4: Trigger mode						
						
	/**************************** Magnetometer ************************************/
	#define LSM303DLHC_MAG_TEMP_DIS					�¶ȴ���������
	#define LSM303DLHC_MAG_TEMP_EN					�¶ȴ���������

	#define LSM303DLHC_MAG_RATE_0_75HZ				Ƶ��Ϊ0.75HZ
	#define LSM303DLHC_MAG_RATE_1_5HZ				Ƶ��Ϊ1.5HZ
	#define LSM303DLHC_MAG_RATE_3HZ					Ƶ��Ϊ3HZ
	#define LSM303DLHC_MAG_RATE_7_5HZ				Ƶ��Ϊ7.5HZ
	#define LSM303DLHC_MAG_RATE_15HZ				Ƶ��Ϊ15HZ
	#define LSM303DLHC_MAG_RATE_30HZ				Ƶ��Ϊ30HZ
	#define LSM303DLHC_MAG_RATE_75HZ				Ƶ��Ϊ75HZ
	#define LSM303DLHC_MAG_RATE_220HZ				Ƶ��Ϊ220HZ

	#define LSM303DLHC_MAG_GRANGE_1_3G				Range�ķ�ΧΪ1.3G
	#define LSM303DLHC_MAG_GRANGE_1_9G				Range�ķ�ΧΪ1.9G
	#define LSM303DLHC_MAG_GRANGE_2_5G				Range�ķ�ΧΪ2.5G
	#define LSM303DLHC_MAG_GRANGE_4G				Range�ķ�ΧΪ4G
	#define LSM303DLHC_MAG_GRANGE_4_7G				Range�ķ�ΧΪ4.7G
	#define LSM303DLHC_MAG_GRANGE_5_6G				Range�ķ�ΧΪ5.6G
	#define LSM303DLHC_MAG_GRANGE_8_1G				Range�ķ�ΧΪ8.1G

	#define LSM303DLHC_MAG_MODE_CON					ģʽΪ����ת��ģʽ
	#define LSM303DLHC_MAG_MODE_SINGLE				ģʽΪ��ת��ģʽ
	#define LSM303DLHC_MAG_MODE_SLEEP				ģʽΪ����ģʽ (ֵΪ2)
	#define LSM303DLHC_MAG_MODE_SLEEP				ģʽΪ����ģʽ (ֵΪ3)

	#define LSM303DLHC_MAG_NOTLOCK					��������Ĵ���δ����
	#define LSM303DLHC_MAG_LOCK						��������Ĵ���������

	#define LSM303DLHC_MAG_NOTDRDY					û�п��õ�����
	#define LSM303DLHC_MAG_DRDY						�п��õ�������
	
�����ӿ�:
	reg					�Ĵ����ӿ�, �ɲ鿴���мĴ�����ֵ���޸��ض��Ĵ�����ֵ
						���������ʽ: "0x%02x:0x%02x" 				(����: "0x02:0xef")
						read(L3G_GET_PATH(reg), buf, 100);			��ȡȫ���Ĵ�����ֵ
																	����ֵ֮���Կո�ָ�(ע��: buf����̫С)
						write(L3G_GET_PATH(reg), "0x02:0xef", 10);	�޸ļĴ���0x02ֵΪ0xef 
	
	delay				�ӳٽӿ�				
						���������ʽ: "%d"							(����: "50")
			
	position			������ٶ�ƫ��ֵ
						���������ʽ: "%d", Ĭ��ֵΪ-3
	
	fuzz				δ֪
	
	temp_en				��ȡ/�����¶ȴ�������״̬
						���������ʽ: "%d", �������ֵΪ(0-1)
	
	sample				��ȡ/���������������
						���������ʽ: "%d", �������ֵΪ(0-7)
						0: 0.75HZ, 1: 1.5HZ, 2: 3HZ,  3: 7.5HZ
						4: 15HZ,   5: 30HZ,  6: 75HZ, 7: 220HZ

	range				��ȡ/��������ѡ��
						���������ʽ: "%d", �������ֵΪ(1-7)
						1: 1.3G,	2: 1.9G,	3: 2.5G,	4: 4G
						5: 4.7G,	6: 5.6G,	7: 8.1G
	
	mode				��ȡ/���ô���������ģʽ״̬
						���������ʽ: "%d", �������ֵΪ(1-3)
						0: ����ת��ģʽ		1: ��ת��ģʽ
						2: ����ģʽ			3: ����ģʽ
	
	mag					��ȡ����������������ֵ
						�����ʽ: "%d %d %d"
	
	lock				��ȡ��������Ĵ���������״̬
						�����ʽ: "%d", ���ֵΪ(0-1)
						0: δ��,			1: ����
	
	drdy				��ȡ���ݾ���״̬
						�����ʽ: "%d", ���ֵΪ(0-1)
						0: ����Ч����,			1: ������
	
	temperature			��ȡ�¶ȴ���������ֵ
						�����ʽ: "%d"