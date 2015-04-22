---
layout: post
title: "Rust and OpenGL: A Tale of Two Bindings"
date: 2013-12-19 17:22
comments: true
categories: [rust, opengl]
published: false
---

Rust is proving to be a strong *blah*.

Tradeoffs. But IMHO bindings should provide a natural interface to the host
language, and hide all the glue.

+        gl::BindBuffer(gl::ARRAY_BUFFER, *particleBuffer);
-        gl2::bind_buffer(gl2::ARRAY_BUFFER, particleBuffer);

macro_rules! to_glstr {
    ($rs:expr) => {
        $rs.to_c_str().unwrap() as *GLchar
    }
}


-        let particleBuffer: gl2::GLuint = 0;
-        gl2::gen_buffers(1);
-        gl2::bind_buffer(gl2::ARRAY_BUFFER, particleBuffer);
-        gl2::buffer_data(gl2::ARRAY_BUFFER, self.emitter.particles, gl2::STATIC_DRAW);
+        let particleBuffer: *mut GLuint;
+        let particle_sz = mem::size_of::<Particle>();
+
+        gl::GenBuffers(1, particleBuffer);
+        gl::BindBuffer(gl::ARRAY_BUFFER, *particleBuffer);
+        gl::BufferData(gl::ARRAY_BUFFER, (NUM_PARTICLES*particle_sz) as GLsizeiptr, cast::transmute(self.emitter.particles), gl::STATIC_DRAW);




-        gl2::vertex_attrib_pointer_f32(self.shader.a_pSizeOffset, 1, false, particle_sz, 16);
-        gl2::vertex_attrib_pointer_f32(self.shader.a_pColourOffset, 3, false, particle_sz, 20);
+        gl::VertexAttribPointer(self.shader.a_pID, 1, gl::FLOAT, gl::FALSE, particle_sz, cast::transmute(0));
+        gl::VertexAttribPointer(self.shader.a_pRadiusOffset, 1, gl::FLOAT, gl::FALSE, particle_sz, 



-        self.a_pID = gl2::get_attrib_location(self.program, "a_pID") as u32;
-        self.a_pRadiusOffset = gl2::get_attrib_location(self.program, "a_pRadiusOffset") as u32;
-        self.a_pVelocityOffset = gl2::get_attrib_location(self.program, "a_pVelocityOffset") as u32;
-        self.a_pDecayOffset = gl2::get_attrib_location(self.program, "a_pDecayOffset") as u32;
-        self.a_pSizeOffset = gl2::get_attrib_location(self.program, "a_pSizeOffset") as u32;
-        self.a_pColourOffset = gl2::get_attrib_location(self.program, "a_pColourOffset") as u32;
+        self.a_pID = gl::GetAttribLocation(self.program, to_glstr!("a_pID")) as u32;
+        self.a_pRadiusOffset = gl::GetAttribLocation(self.program, to_glstr!("a_pRadiusOffset")) as u32;
+        self.a_pVelocityOffset = gl::GetAttribLocation(self.program, to_glstr!("a_pVelocityOffset")) as u32;
+        self.a_pDecayOffset = gl::GetAttribLocation(self.program, to_glstr!("a_pDecayOffset")) as u32;
+        self.a_pSizeOffset = gl::GetAttribLocation(self.program, to_glstr!("a_pSizeOffset")) as u32;
+        self.a_pColourOffset = gl::GetAttribLocation(self.program, to_glstr!("a_pColourOffset")) as u32;



