\documentclass[12pt]{article}

\topmargin -15mm

\textheight 24truecm   
\textwidth 16truecm    
\oddsidemargin 5mm
\evensidemargin 5mm   
\setlength\parskip{10pt}
\pagestyle{empty}          

\usepackage[utf8]{inputenc}
\usepackage{boxedminipage}
\usepackage{amsfonts}
\usepackage{amsmath} 
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{amsthm}
\usepackage{t1enc}
\usepackage{subfig}
\usepackage{bbm}

\let\algorithm\relax
\let\endalgorithm\relax
\usepackage[linesnumbered,ruled,vlined]{algorithm2e}%[ruled,vlined]{
\usepackage{algpseudocode}
\renewcommand{\algorithmicrequire}{\textbf{Input:}}  % Use Input in the format of Algorithm
\renewcommand{\algorithmicensure}{\textbf{Output:}} % Use Output in the format of Algorithm 

\newtheorem{theorem}{Theorem}[section]
\newtheorem{lemma}[theorem]{Lemma}
\newtheorem{proposition}[theorem]{Proposition}
\newtheorem{corollary}[theorem]{Corollary}
\newtheorem{definition}[theorem]{Definition}
\newtheorem{example}[theorem]{Example}
\def\P{\mathbb{P}}
\def\D{\mathbb{D}}
\def\R{\mathbb{R}}
\def\E{\mathbb{E}}
\def\S{\mathcal{S}}
\def\M{\mathcal{M}}

\title{EA MAP513 4.1 : \'Evaluation et gestion des risques}
\author{Xinkai TANG}
\date{March 2021}

\begin{document}

\maketitle

\section{Introduction}
This report focuses on reproducing the results in the paper\cite{ref2}, where Ahdida et al. introduced some simulation methods for Wishart processes and more generally for affine diffusions on positive semidefinite matrices. First a brief introduction to Wishart process is given and the organisation of the report follows after.

Wishart processes are named because of the marginal laws follow Wishart distributions, whose values are symmetric positive matrices. In this report, such processes solve the following SDE:
\begin{equation}
X_{t}^x = x + \int_0^t(\bar{\alpha}+B(X_S^x))\mathbf{d}s+\int_0^t(\sqrt{X_S^x}\mathbf{d}W_Sa+a^T\mathbf{d}W_S^T\sqrt{X_S^x}).
\end{equation}
with $(W_t,t\geq0)$ denotes a $d$-by-$d$ square matrix make of independent standard Brownian motions and
\begin{equation}
x,\bar{\alpha}\in\S_d^+(\R),\quad a\in\M_d(\R)\quad and\quad B\in\mathcal{L}(\S_d(\R))
\end{equation}
is a linear mapping on $\mathcal{S}_d(\R)$. Wishart process correspond to the case where 
$$
\exists\alpha\geq 0, \bar{\alpha} = a^Ta\quad and
$$
\begin{equation}
\exists b\in\M_d(\R),\forall x\in\S_d(\R) \quad B(x)=bx+xb^T.
\end{equation}
Here some natations for real matrices are given:
\begin{itemize}
\item For $d,d'\in\N^*,\M_d(\R)$ denotes the real $d$ square matrices and $\M_{d\times d'}(\R)$ the real matrices with $d$ rows and $d'$ columns.
\item $\S_d(\R),\S_d^+(\R),\S_d^{+,*}$ and $\mathcal{G}_d(\R)$ denote respectively, the set of symmetric, symmetric positive semidefinite, symmetric positive definite and nonsingular matrices.
\item For $x\in\S_d^+(\R)$,$\sqrt{x}$ denotes the unique symmetric positive semidefinite matrix such that $(\sqrt{x})^2=x$.
item The identity matrix is denoted by $I_d$ and we set for $n\leq d, I_d^n = (\mathbbm{1}_{i=j\leq n})_{1\leq i,j\leq d}$ and $e_d^n =(\mathbbm{1}_{i=j=n})_{1\leq i,j\leq d}$.
\item For $\lambda_1,...,\lambda_d\in\R$, diag($\lambda_1,...,\lambda_d$) denotes the diagonal matrix such that\\ diag($\lambda_1,...,\lambda_d)_{i,i}=\lambda_i$.
\end{itemize}

In the prevoius work, I have done some research on the Cox-Ingersoll-Ross (CIR) process introduced in the paper\cite{ref1}, which talks about high order discretization schemes for the CIR process and its application to Heston models. Based on this previous work, I did research on the Wishart process, which is a multi-dimensional extension of the CIR process. Based on Ahdida et al's work I simulated the Wishart process by exact and high-order discretization schemes. The first part of the report consists of a summary of Ahdida's work. In the second part, numerical results are compared and are shown to prove the advantages of the high-order schemes. Also, an application in finance to the Gourieroux and Sufana model\cite{ref3} is also introduced. An analysis of executing time and converging rate is also given.

\section{Simulation of Wishart process}
\subsection{\small Some propertites of affine processes on positive semidefinite matrices}
Before introducing the properties, we define the infinitesimal generator for the affine process by the following formula:
\begin{equation}
x\in\S_d^+(\R),\quad L^{\mathcal{M}}f(x)=\lim_{t\to 0^+}\frac{\E[f(X_t^x)]-f(x)}{t}
\end{equation}
where $f\in \mathcal{C}^2(\mathcal{M}_d(\R),\R)$ with bounded derivatives.
\begin{proposition}
The infinitesimal generator on $\S_d(\R)$ associated to $\mathrm{AFF}_d(x,\bar{\alpha},B,a)$ is given by
\begin{equation}
L^{\S} = \mathrm{Tr}([\bar{\alpha}+B(x)]D^{\S}) +2\mathrm{Tr}(xD^{\S}a^TaD^{\S}),
\end{equation}
where $D^{\S}$ is defined by $D_{i,j}^{\S} = (\mathbbm{1}_{i=j}+\frac{1}{2}\mathbbm{1}_{i\neq j})\partial_{i,j}$, for $i\leq i,j\leq d.$
\end{proposition}

We denote by $\mathrm{AFF}_d(x,\bar{\alpha},B,a)$ the law of $(X_t^x)_{t\geq 0}$ and $\mathrm{AFF}_d(x,\bar{\alpha},B,a;t)$ the marginal law of $X_t^x$. We observe that the infinitesimal generator only depends on $a$ through $a^Ta$ and we can get
\begin{equation}
\mathrm{AFF}_d(x,\bar{\alpha},B,a) \overset{Law}{=} \mathrm{AFF}_d(x,\bar{\alpha},B,\sqrt{a^Ta})
\end{equation}

\begin{proposition}
Let $n=\mathrm{Rk}(a)$ be the rank of $a^Ta$. Then there exist a diagonal matrix $\bar{\delta}$ and a nonsingular matrix $u\in\mathcal{G}_d(\R)$ such that $\bar{\alpha} = u^T\bar{\delta}u$ and $a^Ta = u^TI_d^nu$ and we have
\begin{equation}
\mathrm{AFF}_d(x,\bar{\alpha},B,a)\overset{Law}{=}u^T\mathrm{AFF}_d((u^{-1})^Txu^{-1},\bar{\delta},B_u,I_d^n)u,
\end{equation}
where $\forall y\in \S_d(\R),B_u(y)=(u^{-1})^TB(u^Tyu)u^{-1}$.
\end{proposition}
This proposition gives us a general way to compute $u$ and $\bar{\delta}$. In the case of Wishart process, $u$ can be directly be obtained by using a single extended Cholesky decomposition given by following lemma.
\begin{lemma}
Let $q\in\S_d^+(\R)$ be a matrix with rank $r$. Then there is a permutation matrix $p$, an invertible lower triangular matrix $c_r\in\mathcal(G)_r(\R)$ and $k_r\imathcal{M}_{d-r\times r}(\R)$ such that
$$
pqp^T = cc^T,\quad c= 
\begin{pmatrix}
c_r & 0 \\
k_r & 0
\end{pmatrix}.
$$
The triplet $(c_r,k_r,p)$ is called an extended Cholesky decomposition of $q$. Besides, $\tilde{c} = \begin{pmatrix}
c_r & 0 \\
k_r & I_{d-r}
\end{pmatrix} \in \mathcal{G}_d(\R)$, and we have
$$
q = (\tilde{c}^Tp)^TI_d^r\tilde{c}^Tp.
$$
\end{lemma}
Here we also give the pseudocode of the extended cholesky decomposition.
\begin{algorithm}
    \caption{Extended Cholesky decomposition}
    
    \KwIn{A symmetric positive semidefinite matrix $M\in\S_d^+(\R)$.}
    \KwOut{Extended Cholesky decomposition $(p,k_r,c_r)$.}
    r = 0
    
    \For{$k = 1:n$} {
    Find $q (k\leq q\leq n)$ so $M(q,q) = \mathrm{max}\{M(k,k),...,M(n,n)\}$
    
    \If{$M(q,q)>0$}
    {$r=r+1$
    
    $piv(k)=q$
    
    $M(k,:) \leftrightarrow M(q,:)$
    
    $M(:,k) \leftrightarrow M(:,q)$
    
    $M(k,k) = \sqrt{M(k,k)}$
    
    $M(k+1:n,k) = M(k+1:n,k)/M(k,k)$
    
    \For{$j=k+1:n$} {
    $M(j:n,j) = M(j:n,j)-M(j:n,k)M(j,k)$
    }
    }
    }
    $cr = M[:r,:r]$,$kr = M[r:,:r]$
    
    \Return $(p,k_r,c_r)$;
\end{algorithm}


We get another interesting identity on the marginal laws.
\begin{proposition}
\label{prop1}
Let $t > 0,a,b\in\mathcal{M}_d(\R)$ and $\alpha\geq d-1$. Let $m_t=\mathrm{exp}(tb), q_t = \int_0^t\mathrm{exp}(sb)a^Ta\mathrm{exp}(sb^T)ds$ and $n=\mathrm{Rk}(q_t)$. Then there is $\theta_t\in\mathcal{G}_d(\R)$ such that $q_t = t\theta_tI_d^n\theta^T_t$, and we have
\begin{equation}
\mathrm{WIS}_d(x,\alpha,b,a;t) \overset{Law}{=}\theta_t\mathrm{WIS}(\theta_t^{-1}m_txm_t^T(\theta_t^{-1})^T,\alpha,0,I_d^n;t)\theta_t^T.
\end{equation}
\end{proposition}
Thanks to this proposition, we can sample any Wishart distribution if we are able to simulate exactly the distribution $\mathrm{WIS}(x,\alpha,0,I_d^n;t)$ for any $x\in\S_d^+(\R)$. Also, here we can compute the matrix $\theta_t$ by using the extended Cholesky decomposition of $q_t/t$.


\subsection{Exact simulation of Wishart process}
In this section, we will introduce an exact simulation method for noncentral Wishart distribution that works for any $\alpha\geq d-1$. Thanks to Propostion 2.4, we can focus on the case $b=0,a=I_d^n$. The following theorem explains how to split the infinitesimal generator of $\mathrm{WIS}_d(x,\alpha,0,I_d^n)$ as the sum of commutative infinitesimal generators.
\begin{theorem}
\label{thm1}
Les L be the generator associated to the Wishart process $\mathrm{WIS}_d(x,\alpha,0,I_d^n)$ and $L_{e^i_d}$ be the generator associated to $\mathrm{WIS}(x,\alpha,0,e^i_d)$ for $i\in \{1,...,d\}.$ Then we have
\begin{equation}
L=\Sigma_{i=1}^n L_{e^i_d}\quad and\quad \forall i,j\in\{1,...,d\}\quad L_{e^i_d}L_{e^j_d}=L_{e^j_d}L_{e^i_d}
\end{equation}
\end{theorem}
We can notice that the process $\mathrm{WIS}_d(x,\alpha,0,e^i_d)$ and $\mathrm{WIS}(x,\alpha,0,I_d^n)$ are well defined on $\S_d^+(\R)$ under the same hypothesis which is $\alpha\geq d-1$ and $x\in\S_d^+(\R).$
\begin{definition}
Consider $t>0$ and $x\in\S_d^+(\R)$. We define
$$
X_t^{1,x}\sim\mathrm{WIS}_d(x,\alpha,0,e^1_d;t),
$$
$$
X_t^{2,X_t^{1,x}}\sim\mathrm{WIS}_d(X_t^{1,x},\alpha,0,e^2_d;t),
$$
$$
...
$$
$$
X_t^{n,...^{X_t^{1,x}}}\sim\mathrm{WIS}_d(X_t^{n-1,...^{X_t^{1,x}}},\alpha,0,e^n_d;t).
$$
\end{definition}
Conditionally to $X_t^{i-1,...^{X_t^{1,x}}},X_t^{i,...^{X_t^{1,x}}}$ is sampled according to the distribution at time $t$ of a Wishart process starting from $X_t^{i-1,...^{X_t^{1,x}}}$ and with parameters $(\alpha,0,e_d^i)$. We have the following result.
\begin{proposition}
Let $X_t^{n,...^{X_t^{1,x}}}$ be defined as above. Then
$$
X_t^{n,...^{X_t^{1,x}}}\sim\mathrm{WIS}_d(x,\alpha,0,I_d^n;t).
$$
\end{proposition}
Thanks to this proposition, we can simulate $\mathrm{WIS}_d(x,\alpha,0,I_d^n;t)$ as soon as we can simulate $\mathrm{WIS}_d(x,\alpha,0,e_d^i;t)$. These laws are the same as $\mathrm{WIS}_d(x,\alpha,0,e^1_d;t)$, up to the permutation of the first and $i$th coordinates.

Now we present a general way to sample exactly $\mathrm{WIS}_d(x,\alpha,0,e_d^1;t)$ in Algorithm 2. Based on that, we are able to simulate $\mathrm{WIS}_d(x,\alpha,0,e^k_d;t)$ for $k\in\{1,...,d\}$ and $\mathrm{WIS}_d(x,\alpha,0,I^n_d;t)$, which is given in Algorithm 3. Then we have an exact scheme for $\mathrm{WIS}_d(x,\alpha,b,a;t)$.
\begin{algorithm}
    \caption{Exact simulations for $\mathrm{WIS}(x,\alpha,0,e^1_d;t)$}
    
    \KwIn{$x\in\S_d^+(\R),d,\alpha\geq d-1$ and $t>0$.}
    \KwOut{$X$, sampled according to $\mathrm{WIS}_d(x,\alpha,0,e^1_d;t)$.}
    Compute the extended Cholesky decomposition $(p,k_r,c_r)$ of $(x_{i,j})_{2\leq i,j\leq d}$;
    
    Set $\pi=\begin{pmatrix} 1 & 0 \\ 0 & p \end{pmatrix}, \tilde{x} = \pi x\pi^T,(u_{\{1,l+1\}})_{1\leq l\leq r}$ and $u_{\{1,1\}} = \tilde{x}_{\{1,1\}}-\Sigma_{k=1}^r(u_{\{1,k+1\}})^2\geq 0$;
    
    Sample independently $r$ normal variables $G_2,...,G_{r+1}\sim\mathcal{N}(0,1)$ and $(U_t^u)_{\{1,1\}}$ as a CIR process at time t starting from $u_{\{1,1\}}$ solving $d(U_t^u)_{\{1,1\}}=(\alpha -1)dt+2\sqrt{(U_t^u){\{1,1\}}}dZ_t^1$;
    
    Set $(U_t^u)_{\{1,1\}} = u_{\{1,l+1\}} + \sqrt{t}G_{l+1}$.
    
    \Return{\begin{equation*}
    \begin{split}
    X&=\pi^T\begin{pmatrix} 1 & 0 & 0\\ 0 & c_r & 0\\ 0 & k_r & I_{d-r-1} \end{pmatrix} \\
    &\times\begin{pmatrix} (U_t^u)_{\{1,1\}}+\Sigma_{k=1}^r((U_t^u)_{\{1,k+1\}})^2 & ((U_t^u)_{\{1,l+1\}})^T_{1\leq l\leq r} & 0\\ ((U_t^u)_{\{1,l+1\}})_{1\leq l\leq r} & I_r & 0\\ 0 & 0 & 0 \end{pmatrix}\\
    &\times\begin{pmatrix} 1 & 0 & 0\\ 0 & c_r^T & k_r^T\\ 0 & 0 & I_{d-r-1} \end{pmatrix}\pi
    \end{split}
    \end{equation*}}
\end{algorithm}

\begin{algorithm}
    \caption{Exact simulations for $\mathrm{WIS}(x,\alpha,0,I^n_d;t)$}
    
    \KwIn{$x\in\S_d^+(\R),n\leq d,\alpha\geq d-1$ and $t>0$.}
    \KwOut{$X$, sampled according to $\mathrm{WIS}_d(x,\alpha,0,I^n_d;t)$.}
    $y=x$;
    
    \For{$k=1:n$} {
    Set $p_{k,1} = p_{1,k} = p_{i,i} = 1$ for $i\not\in\{1,k\}$ and $p_{i,j} = 0$ otherwise.
    
    $y=pYp$ where $Y$ is sampled according to $\mathrm{WIS}_d(pyp,\alpha,0,e^1_d;t)$ by using Algorithm2.
    }
    \Return $X=y$.
\end{algorithm}

\begin{algorithm}
    \caption{Exact simulations for $\mathrm{WIS}(x,\alpha,b,a;t)$}
    
    \KwIn{$x\in\S_d^+(\R),\alpha\geq d-1$, $a,b\in\mathcal{M}_d(\R)$ and $t>0$.}
    \KwOut{$X$, sampled according to $\mathrm{WIS}_d(x,\alpha,b,a;t)$.}
    Calculate $q_t = \int_0^t\mathrm{exp}(sb)a^Ta\mathrm{exp}(sb^T)ds$ and $(p,c_n,k_n)$ an extended Cholesky decomposition of $q_t/t$;
    
    Set $\theta_t = p^{-1}\begin{pmatrix} c_n & 0 \\ k_n & I_{d-n} \end{pmatrix}$ and $m_t = \mathrm{exp}(tb)$.
    
    \Return $X=\theta_tY\theta_t^T$, where $Y\sim\mathrm{WIS}_d(\theta_t^{-1}m_txm_t^T(\theta_t^{-1})^T,\alpha,0,I_d^n;t)$ is sampled by Algorithm 3.
\end{algorithm}
\newpage
\subsection{High-order discretization shcemes for Wishart process}
In this section we introduce a way to get eak $\nu$-th order schemes for any Wishart processes. First we obtain a $\nu$th-order scheme for $\mathrm{WIS}_d(x,\alpha,0,e^1_d)$. Then we get a $\nu$th-order scheme for $\mathrm{WIS}_d(x,\alpha,0,I_d^n)$. Last, we use the identity law to get a weak $\nu$th-order scheme for any Wishart process.
\begin{proposition}
\label{prop2}
Let $L_1,L_2$ be the generator of SDEs defined on $\D$. Let $\hat{X}_t^{1,x}$ and $\hat{X}_t^{2,x}$ denote two potential weak $\nu$th-order schemes on $\D$ for $L_1$ and $L_2$.
\begin{enumerate}
\item  If $L_1L_2=L_2L_1,\hat{X}_t^{2,\hat{X}_t^{1,x}}$ is a potential weak $\nu$th-order discretization scheme for $L_1+L_2$.
\item Let B be an independent Bernoulli variable of parameter 1/2. If $\nu\geq 2$,
$$
(a)\quad B\hat{X}_t^{2,\hat{X}_t^{1,x}} + (1-B)\hat{X}_t^{1,\hat{X}_t^{2,x}}\quad and\quad (b)\quad \hat{X}_{t/2}^{2,\hat{X}_{t}^{1,\hat{X}_{t/2}^{2,x}}}
$$
are potential weak second-order schemes for $L_1+L_2$.
\end{enumerate}
\end{proposition}
Now we start from giving a potential weak $\nu$th-order scheme for $\mathrm{WIS}_d(x,\alpha,0,I_d^n)$.
\begin{definition}
Let $u$ be a matrix, $(c_r(u),k_r(u),I_{d-1})$ is the extended Cholesky decomposition of  $(u_{i,j})_{2\leq i,j\leq d}$. Then we define the function $h_r(u)$
\begin{equation*}
    \begin{split}
    h_r(u)&=\begin{pmatrix} 1 & 0 & 0\\ 0 & c_r(u) & 0\\ 0 & k_r(u) & I_{d-r-1} \end{pmatrix} \\
    &\times\begin{pmatrix} u_{\{1,1\}}+\Sigma_{k=1}^r(u_{\{1,k+1\}})^2 & (u_{\{1,l+1\}})^T_{1\leq l\leq r} & 0\\ (u_{\{1,l+1\}})_{1\leq l\leq r} & I_r & 0\\ 0 & 0 & 0 \end{pmatrix}\\
    &\times\begin{pmatrix} 1 & 0 & 0\\ 0 & c_r(u)^T & k_r(u)^T\\ 0 & 0 & I_{d-r-1} \end{pmatrix}
    \end{split}
    \end{equation*}
\end{definition}
\begin{theorem}
\label{thm2}
Let $x\in\S_d^+(\R)$ and $(c_r,k_r,p)$ be an extended Cholesky decomposition of $(x_{i,j})_{2\leq i,j\leq d}.$ We set $\pi=\begin{pmatrix}1 & 0 \\0 & p\end{pmatrix}$ and $\tilde{x} = \pi x\pi^T$, so that $(\tilde{x}_{i,j})_{2\leq i,j\leq d} = \begin{pmatrix}c_r & 0 \\k_r & 0\end{pmatrix}\begin{pmatrix}c_r^T & k_r^T \\0 & 0\end{pmatrix}$. We set
$$
u_{\{1,1\}} = \tilde{x}_{\{1,1\}} - \Sigma_{k=1}^{r}(u_{\{1,k+1\}})^2\geq 0,
$$
where
$$
(u_{\{1,l+1\}})_{1\leq l\leq r} = c_r^{-1}(\tilde{x}_{\{1,l+1\}})_{1\leq l\leq r},
$$
and we set $u_{\{1,i\}} = 0$ if $r+2\leq i\leq d$ and $u_{\{i,j\}} = \tilde{x}_{\{i,j\}}$ if $i,j\geq 2$. Let $(\hat{G}^i)){1\leq i\leq r}$ be a sequence of independent real variables with finite moments of any order such that
$$
\forall i\in \{1,...,r\},\forall k\leq 2\nu+1\quad \E[(\hat{G}^i)^k] = \E[G^k], G\sim\mathcal{N}(0,1).
$$
Let $h_r$ be the function defined as above. Let $(\hat{U}_t^u)_{\{1,1\}}$ be sampled independently according to a potential weak $\nu$th-order scheme for the CIR process $d(U_t^u)_{\{1,1\}} = (\alpha-r)dt + 2\sqrt{(U_t^u)_{\{1,1\}}}dZ_t^1$ starting from $u_{\{1,1\}}$. We set
\begin{equation*}
    \begin{split}
    (\hat{U}_t^u)_{\{1,i\}}&=u_{\{1,i\}}+\sqrt{t}\hat{G}^i,\quad 2\leq i\leq r+1,\\
    (\hat{U}_t^u)_{\{1,i\}}&=0,\quad r+2\leq i\leq d,\\
    (\hat{U}_t^u)_{\{i,j\}}&=u{\{i,j\}}\quad if\quad i,j\geq 2.
    \end{split}
\end{equation*}
Then, the scheme $\hat{X}_t^x = \pi^Th_r(\hat{U}_t^u)\pi$ is a potential $\nu$th-order scheme for $L_{e^1_d}$ and takes values in $\S_d^+(\R)$.
\end{theorem}

Based on Theorem \ref{thm2}, we can simulate a potential weak $\nu$th-order scheme for $\mathrm{WIS}_d(x,\alpha,0,e^1_d)$. For $i\in\{2,...,d\},\mathrm{WIS}_d(x,\alpha,0,e^i_d)$ and $\mathrm{WIS}_d(x,\alpha,0,e^1_d)$ ahve the same law up to the premutation of the first and $i$th coordinate. Let $\pi^{1\leftrightarrow i}$ denote the associated permutation matrix. Then we have
$$
\hat{X}_t^{i,x} = \pi^{1\leftrightarrow i}\hat{X}_t^{1,\pi^{1\leftrightarrow i}x\pi^{1\leftrightarrow i}}\pi^{1\leftrightarrow i}
$$
is a potential $\nu$th-order scheme for $\mathrm{WIS}_d(x,\alpha,0,e^i_d)$. Last, we get from prevoius Theorem \ref{thm1} and Proposition \ref{prop2} that $\hat{X}_t^{n,...^{\hat{X}_t^{1,x}}}$ is a potential weak $\nu$th order scheme for WIS$_d(x,\alpha,0,I_d^n)$. Thanks to Proposition \ref{prop1}, we are able to construct a scheme for any Wishart process $\mathrm{WIS}_d(x,\alpha,b,a)$.

Also, we introduce another scheme wich is served as a standard one. We consider the following corrected Euler-Maruyama scheme for $\mathrm{AFF}_d(x,\bar{\alpha},B,a)$:
\begin{equation}
\hat{X}^N_{t_0^N} = x,
\end{equation}
\begin{equation*}
    \begin{split}
    \hat{X}_{t^N_{i+1}}^N&=\hat{X}_{t^N_i}^N+(\bar{\alpha}+B(\hat{X}_{t^N_i}^N))\frac{T}{N}+\sqrt{(\hat{X}_{t^N_i}^N)^+}(W_{t_{i+1}^N}-W_{t_i^N})a\\
    &+a^T(W_{t_{i+1}^N}-W_{t_i^N})^T\sqrt{(\hat{X}_{t^N_i}^N)^+}.\\
    \end{split}
\end{equation*}
Here, $x^+$ denotes the matrix that has the same eigenvectors as $x$ with the same eigenvalue if it is positive and a zero eigenvalue otherwise. More precisely, $x^+ = o\mathrm{diag}(\lambda_1^+,...,\lambda_d^+)o^T$ for $x = o\mathrm{diag}(\lambda_1,...,\lambda_d)o^T$.
\subsection{An application in finance}
We will present here a possible application of the schemes discussed above. The model we consider here is introduced by Gourieroux and Sufana, which is a model for $d$ risky assets $S_t^1,...,S_d^t$. Let $(B_t,t\geq 0)$ denote a standard Brownian motion on $\R^d$ that is independent from $(W_t,t\geq 0)$. Then we consider the following dynamics for the assets:
\begin{equation}
t\geq 0, 1\leq l \leq d,\quad S_t^l = S_0^l+r\int^t_0 S_u^l du+\int_0^t S_u^l(\sqrt{X_u}dB_u)_l,
\end{equation}
where $X_t=X_0+\int_0^t(\alpha a^Ta+bX_u+X_ub^T)du+\int_0^t(\sqrt{X_u}dW_ua+a^TdW_u^T\sqrt{X_u})$ is a Wishart process. $(\sqrt{X_u}dB_u)_l$ is simply the $l$th coordinates of the vector $\sqrt{X_u}dB_u$. We can easily check that the instantaneous quadratic covariation matrix between the log-prices of the assets is $X_t$, and $r$ denotes the instantaneous interest rate.

In practice, we can use $S_t^l = S_0^l\mathrm{exp}[(r-x_{l,l}/2)t+(cB_t)_l]$ where $c$ is computed with an extended Cholesky decomposition of $x$ to simulate $S_t^l$. Then by the Proposition \ref{prop2} we can take the second-order scheme for $\mathrm{WIS}_d(x,\alpha,b,a)$ and the exact scheme for $L^S$. This gives us a second-order scheme. Also, we consider the Euler-Maruyama scheme defined as follows:
$$
\hat{S}_{t_0^N}^{l,N} = S_0^l,
$$
$$
\hat{S}_{t^N_{i+1}}^{l,N} = \hat{S}_{t^N_i}^{l,N}(1+rT/N+(\sqrt{(\hat{X}^N_{t_i^N})^+}(B^N_{t^N_{i+1}}-B_{t_i^N}))_l),\quad 0\leq i\leq N-1.
$$

\section{Numerical results on the simulation methods}
\subsection{Results on Wishart processes}
In this section we present the numerical results on the convergence and time comparison between different algorithms. We have plotted for each scheme $\E[\mathrm{exp}(-\mathrm{Tr}(iv\hat{X}_{t^N_N}^N))]$ in function of the time step $T/N$. This expectation is calculated by a Monte Carlo method. Each time we will consider a case where $\alpha\geq d$ and a case where $d-1\leq\alpha<d$. In these figures:
\begin{itemize}
\item scheme 1 denotes the value obtained by the exact scheme with one time-step,
\item scheme 2 denotes the second-order scheme,
\item scheme 3 denotes the third-order scheme,
\item scheme 4 is the corrected Euler scheme.
\end{itemize}
We present the numericals results with different parameters to show the correctness of different algorithms. From Figure \ref{fig1} we can see the convergences that fit the theoretical results, which proves that Algorithms 2 and 3 are correct. We also notice that when $\alpha>d$, the simulation gives us satisfying results, while when $d-1\leq \alpha\leq d$, which is much tougher, the results is not so satisfying, as we can see in the Up-right in Figure 1.

\begin{figure}[htbp]
\centering
\subfigure{
\begin{minipage}[t]{0.45\linewidth}
\centering
\includegraphics[width=\textwidth]{e_1.png}
%\caption{fig1}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.45\linewidth}
\centering
\includegraphics[width=\textwidth]{e_2.png}
%\caption{fig2}
\end{minipage}%
}%
\subfigure{
\begin{minipage}[t]{0.45\linewidth}
\centering
\includegraphics[width=\textwidth]{i_1.png}
%\caption{fig2}
\end{minipage}%
}%
\centering
\caption{$d=3,10^6$ Monte Carlo samples, $T=10, v= 0.05I_d$. The real value of $\E[\mathrm{exp}(-\mathrm{Tr}(iv\hat{X}_{t^N_N}^N))]$ as a function of $T/N$. Up-left: $\mathrm{WIS}(x,\alpha,0,e^1_d)$ with $\alpha=4.5$. Up-right: $\mathrm{WIS}(x,\alpha,0,e^1_d)$ with $\alpha=2.22$, Down: $n=3$, $\mathrm{WIS}(x,\alpha,0,I^n_d)$ with $\alpha=4.5$.}
\label{fig1}
\end{figure}
Then, we present the numerical results of a Wishart process. As expected, we observe in Figure 2 that Scheme 2 converges in $O(1/N^2)$ and scheme 3 converges faster in $O(1/N^3)$. Also, the corrected euler scheme converges in $O(1/N)$ speed.

\begin{figure}[htbp]
\centering
\begin{minipage}[t]{0.6\linewidth}
\centering
\includegraphics[width=\textwidth]{f1_left.png}
%\caption{fig1}
\end{minipage}%
\centering
\caption{$d=3,10^6$ Monte Carlo samples of $\mathrm{WIS}(x,\alpha,b,a)$, $T=10$.The real value of $\E[\mathrm{exp}(-\mathrm{Tr}(iv\hat{X}_{t^N_N}^N))]$ as a function of $T/N$. $v=0.05I_d$ and Wishart parameters $x=0.4I_d,\alpha = 4.5,a=I_d,b=0$. Exact value: 0.054277.}
\label{fig2}
\end{figure}
% Right:$v=0.2I_d+0.04q$ and Wishart parameters $x=0.4I_d+0.2q,\alpha=2.22,a=I_d,b=-0.5I_d$. Exact value: 0.239836. Here, $q$ is the matrix defined by: $q_{i,j} = \mathbbm{1}_{i\neq j}$. The width of each point represents the 95\% confidence interval.

\subsection{Results on Gourieroux and Sufana model}
We present in Figure 3 the price of a put option on the maximum of two assets. In Figure 4, we give the exact value and the numerical results for scheme 2. We expect for a quadratic convergence.
\begin{figure}[htbp]
\centering
\begin{minipage}[t]{0.6\linewidth}
\centering
\includegraphics[width=\textwidth]{gs_left.png}
%\caption{fig1}
\end{minipage}%
\centering
\caption{$10^5$ Monte Carlo samples of $\E[e^{-rT}(K-\mathrm{max}(\hat{S}_{t_N^N}^{1,N},\hat{S}_{t_N^N}^{2,N}))]$ in function of $T/N$. $d=2,T=1,K=120,S_0^1=S_0^2=100$, and $r=0.02$. Wishart parameters $x=0.04I_d+0.02q$, with $q_{i,j} = \mathbbm{1}_{i\neq j},a=0.2I_d,b=0.5I_d,\alpha=4.5$. Also we give the 95\% confidence interval. }
\label{fig2}
\end{figure}

\subsection{Time comparison between different algorithms}
Here we will give a small summary of the time cost among different algorithms. The most time-consuming part for these algorithms are the Cholesky decomposition part, which is $O(d^4)$ for one time-step. While in reality, when we do simulations on the computer, we observe that the time cost for scheme 2 and exact scheme is smaller than that of scheme 3, about half quicker. Also, given the dimension $d$ and other parameters, the total time cost is propotional to the time steps $N$. So to conclude, the exact scheme is now the most efficient way to calculate an expectation that only depends on $X_T$.

\subsection{Conclusion}
To conclude, we have implemented the algorithms in the article. And we have replicated the results as in the article. But when dealing with tough cases, namely when $d-1\leq\alpha\leq d$, our implementation works not so well. And also, our performances are not as satisfying as in the article. One of the psossible reasons is lack of simulation time.

\begin{thebibliography}{99}
\bibitem{ref2} Abdelkoddousse Ahdida, Aurélien Alfonsi. "Exact and high-order discretization schemes for Wishart
processes and their affine extensions." The Annals of Applied Probability, 23(3) 1025-1073 June 2013.

\bibitem{ref1} Alfonsi, A. (2010). High order discretization schemes for the CIR process: application to affine term structure and Heston models. Mathematics of Computation, 79(269), 209-237.  

\bibitem{ref3} GOURIEROUX, C. and SUFANA, R. (2003). Wishart quadratic term structure models. Working paper.
\end{thebibliography}

\end{document}