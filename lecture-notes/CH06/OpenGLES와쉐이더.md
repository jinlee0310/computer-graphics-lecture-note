## 1. Vertex and Index Arrays

![image.png](/lecture-notes/CH06/opengles1.png)

## 2. OpenGL ES

- vertex shader, fragment shader이 필요
- glsl이라는 언어로 코딩

## 3. Vertex Shader

- attributes: vertex array를 구성하는 종류(e.g. position, normal, tex coord,…)
- uniforms: remain constant for multiple executioins of a shader. (e.g. world transform, view transform, projection,…)
- gl_Position: clip-space vertex position in vec3 normal

![image.png](/lecture-notes/CH06/opengles2.png)

```glsl
uniform mat3 worldMat, viewMat, projMat;

// attributes
// layout: vertex arrya의 어디에 저장되어있는지 표시
layout(location = 0) in vec3 position;
layout(location = 1) in vec3 normal;
layout(location = 2) in vec2 texCoord;

// outputs
out vec3 v_normal;
out vec2 v_texCoord;

void main(){
	gl_Position = projMat * viewMat * worldMat * vec4(position, 1.0); // clip_Space positions
	// 1. mat3(worldMat): 3*4 부분, 즉 L 부분만 추출
	// 2. inverse-transpose 적용
	// 3. 정규화 적용
	// 4. 기존 normal에 곱해줌
	v_normal = normalize(transpose(inverse(mat3(worldMat))) * normal);
	v_texCoord = texCoord;
}
```

## 4. GL Program

- GL commands begin with the prefix `gl`
- GL data types begin with the prefix `GL`

## 5. Attributes

- buffer objects
  - vertex array: array buffer object, GL_ARRAY
  - index array: element array buffer object

![image.png](/lecture-notes/CH06/opengles3.png)

![image.png](/lecture-notes/CH06/opengles4.png)

- stride=sizeof(vertex)

```glsl
glEnableVertexAttribArray(0); // position = attribute 0, 0번 활성화
// 0번 index, 차원, float type, , stride, start point
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, sizeof(Vertex),(const GLvoid*) offsetof(Vertex,pos))

glEnableVertexAttribArray(1); // normal = attribute 1, 1번 활성화
glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, sizeof(Vertex),(const GLvoid*) offsetof(Vertex,nor))

glEnableVertexAttribArray(2); // texture coordinates = attribute 2, 2번 활성화
glVertexAttribPointer(2, 2, GL_FLOAT, GL_FALSE, sizeof(Vertex),(const GLvoid*) offsetof(Vertex,tex))
```

## 6. Uniforms

### Drawcalls

draw 명령을 내린다.

```glsl
glDrawArrays(GL_TRIANGLES, 0 ,144) // 48 triangles
glDrawElements(GL_TRIANGLES, 144, GL_UNSIGNED_SHORT, 0)
```
