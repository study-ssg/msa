���� MSA - Part1
https://medium.com/coupang-tech/%ED%96%89%EB%B3%B5%EC%9D%84-%EC%B0%BE%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%9A%B0%EB%A6%AC%EC%9D%98-%EC%97%AC%EC%A0%95-94678fe9eb61


����  Legacy Pain Point - ���Ž� ������ ���� �츮 ������ ����� ����

1. �κ��� ��ְ� ��ü�� ���� ��ַ� Ȯ��Ǵ� ������ �ִ�.
 - ����� ��ְ� �ֹ��� ��ַα��� �̾������� �ȵȴ�.
2. Monolithic architecture�� �κ����� Scale-out�� �ϱ� ��ƴ�.
 - �ʿ��� �κ��� Scale-out�� ���� ��ü�� Scale-out�ؾ��Ѵ� ��Ȳ
3. ���� ������Ʈ�� �ϳ��� ���񽺿� ������ ���·� �Ǿ� �־� ������ ������ �ſ� ��ư�, ������ ��� ���⵵�� �ľ��ϱ� �����.
 - �𿩶� �޵��� �ҽ��ڵ� (ex. pd-lib)
4. ���� ���濡�� ���� ������ �׽�Ʈ ����� �߻��Ѵ�.
 - �ֹ��� �����ߴµ� ��� ������ �������� �׽�Ʈ �ؾ��ϴ� ��Ȳ (����Ǿ��ִٸ�)
5. Monolithic architecture���� ����(������/��)�� �����Ҽ���, ������ ��� �ð��� ��������� �����Ѵ�.
 - ex) pd-lib�� ������ ���������� ���� �ڵ尡 ���� ��������� �̿��ؾ���.

������ MSA ����

1. Vitamin Framework�� ���� 
- micro service architecture�� ���� Java ����� framework
- micro service architecture�� ���� ǥ�� skeleton code template�� ����.
- ǥ�� skeleton code template���� ���� ������ ������ front / api / batch / back-office ���񽺸� ���� �����ϱ� ���� �⺻ �ڵ���� ���ԵǾ� ������, ���ο��� ������ �پ��� ǥ�� ���̺귯���鵵 ���� ����� �� �ֵ��� �Ǿ� ����.
- ������������ ���ٸ� ��¾��� �׽�Ʈ, ����, ����͸�, �ڵ� ������ �����ϸ�, Message Queue, Cache �� ��� platform ���񽺿� ���� ����
2. Provider helper library (api-adapter)
- Ư�� API�� ����ϱ� ���Ͽ� ��� client���� HTTP ����ϴ� ����� �����, json ������ API�� ȣ���� �� Object�� �����ϴ� ������ �����Ͽ��� �Ѵ�.
- ������ Provider helper library (api-adapter)�� ���� �����Ͽ� API�� ���� ����� �� �ִ� ������ ���Ͽ���.
3. Message Queue�� �̿��� Transaction �� �и�
- �ֹ� ������ ���� �� ī��ī�� message ���� ����� ī��ī���� ������ polling �Ͽ� ��� ������ ����

������ MSA �÷���
https://medium.com/coupang-tech/%ED%96%89%EB%B3%B5%EC%9D%84-%EC%B0%BE%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%9A%B0%EB%A6%AC%EC%9D%98-%EC%97%AC%EC%A0%95-a31fc2d5a572

1. Configuration Management Database (CMDB)
- ������ CMDB �ý����� ���񽺿� ���񽺸� �����ϴ� ��� �ڿ����� ���� ��Ÿ DB�̸�, Key/Value Collection ���� ������ metadata�� ��ȣ�� ���踦 ������ ����.
- ���� �츮�� �����ϴ� Rancher ���� ��Ȱ�ε�?
2. Coupang Deployment System
- blue / green deployment strategy�� ���� cloud ����� �� ���� �ý���
- 10�� �̳� �ѹ��� �����Ͽ� ���� ��ְ� �߻��ϴ��� ������ ������ ����