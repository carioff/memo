NIO(New IO) API 1.4 
Non-blocking 입출력과 데이터 버퍼링 기능을 사용

NIO API의 네 가지 핵심 요소
1. Buffer - 버퍼를 나타낸다. 기본 데이터 타입에 대한 버퍼가 각각 존재하며 입출력 데이터를 임시로 저장할 때 사용된다.
2. Charset - 캐릭터셋을 나타낸다. 바이트 데이터와 문자 데이터를 인코딩/디코딩할때 사용된다.
3. Channel - 데이터가 통과하는 스트림을 나타낸다. 소켓, 파일, 파이프 등 다양한 입출력 스트림에 대한 채널이 존재한다.
4. Selector - 하나의 쓰레드에서 다중의 채널로부터 들어오는 입력 데이터를 처리할 수 있도록 해 주는 멀티플렉서(multiplexer)이다. 
  논블럭킹 입출력을 위한 핵심 요소이다.
  
java.nio.Buffer	모든 버퍼가 상속받는 추상 클래스
java.nio.ByteBuffer	byte 타입의 데이터를 저장하는 버퍼. 다이렉트(direct) 버퍼와 논다이렉트(nondirect) 버퍼가 존재하며, ReadableByteChannel과 WritableByteChannel을 통해서 데이터를 입출력할 수 있다.
java.nio.MappedByteBuffer	byte를 저장하는 버퍼로서 항상 다이렉트이다. 파일의 특정 영역을 메모리에 매핑시킬 때 사용된다.
java.nio.CharBuffer	char를 저장하며, 다이렉트 또는 논다이렉트 버퍼일 수 있다. 채널에 쓸 수 없다.
java.nio.DoubleBuffer	double을 저장하며, 다이렉트 또는 논다이렉트 버퍼일 수 있다. 채널에 쓸 수 없다.
java.nio.FloatBuffer	float을 저장하며, 다이렉트 또는 논다이렉트 버퍼일 수 있다.
java.nio.IntBuffer	int 데이터를 저장하며, 다이렉트 또는 논다이렉트 버퍼일 수 있다.
java.nio.LongBuffer	int 데이터를 저장하며, 다이렉트 또는 논다이렉트 버퍼일 수 있다.
java.nio.ShortBuffer	int 데이터를 저장하며, 다이렉트 또는 논다이렉트 버퍼일 수 있다.

Direct vs Nondirect
ByteBuffer.allocate()
다이렉트 버퍼를 생성할 때에는 일반적인 자바 배열을 사용하는 경우 메모리를 할당하고 해제할 때 논다이렉트 버퍼에 비해 더 많은 시간이 소비되는 반면에 
원시 메소드를 사용하기 때문에 입출력 처리 속도는 빠르다. 
따라서, 다이렉트 버퍼의 경우는 크고 지속적으로 사용되는 버퍼에 알맞다.

Channel


Ref: https://javacan.tistory.com/tag/MappedByteBuffer
